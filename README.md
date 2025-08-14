# gh-app-spike-test<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="#FFFFFF"
    android:padding="20dp">

    <!-- App Bar -->
    <TextView
        android:layout_width="match_parent"
        android:layout_height="56dp"
        android:background="#FF6600"
        android:gravity="center_vertical|center"
        android:text="ওটিপি যাচাই করুন"
        android:textColor="#FFFFFF"
        android:textSize="20sp" />

    <!-- Icon -->
    <ImageView
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="40dp"
        android:src="@drawable/ic_phone" />

    <!-- Info Text -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="20dp"
        android:text="নির্বাচিত মোবাইল নাম্বারে ওটিপি সম্বলিত এসএমএস পাঠানো হয়েছে"
        android:textSize="16sp"
        android:textColor="#000000"
        android:textAlignment="center" />

    <!-- Phone Number -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="10dp"
        android:text="+88 01728-829902"
        android:textSize="18sp"
        android:textColor="#FF6600" />

    <!-- OTP Boxes (Hint only) -->
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="30dp"
        android:hint="------"
        android:gravity="center"
        android:inputType="number"
        android:textSize="24sp" />

    <!-- Timer -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="end"
        android:layout_marginTop="10dp"
        android:text="পুনরায় ওটিপি পাঠান?  ০০:৫৭"
        android:textSize="14sp"
        android:textColor="#888888" />

    <!-- Verify Button -->
    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="30dp"
        android:text="যাচাই করুন"
        android:textColor="#FF6600"
        android:background="@drawable/rounded_border" />

</LinearLayout>