## MainActivity.java

```java
package com.example.mymessagingapp;

import android.Manifest;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.net.Uri;
import android.os.Bundle;
import android.telephony.SmsManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ListView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    EditText nInp,mInp;
    Button btn;
    ListView lv;
    ArrayList<String> smsList=new ArrayList<>();
    ArrayAdapter<String> adapter;
    final int SMS_REQUEST_CODE=100;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        nInp=findViewById(R.id.nInp);
        mInp=findViewById(R.id.mInp);
        btn=findViewById(R.id.btn);
        lv=findViewById(R.id.lv);
        adapter=new ArrayAdapter<>(this, android.R.layout.simple_list_item_1,smsList);
        lv.setAdapter(adapter);
        if((ActivityCompat.checkSelfPermission(this, Manifest.permission.SEND_SMS)!= PackageManager.PERMISSION_GRANTED)
                ||(ActivityCompat.checkSelfPermission(this, Manifest.permission.READ_SMS)!= PackageManager.PERMISSION_GRANTED)){
            ActivityCompat.requestPermissions(this,new String[]{Manifest.permission.READ_SMS,
                    Manifest.permission.SEND_SMS},SMS_REQUEST_CODE);
        }else{
            readSms();
        }
        btn.setOnClickListener(v->sendSms());
    }
    public void readSms(){
        smsList.clear();
        try {
            Uri uri=Uri.parse("content://sms/inbox");
            Cursor cursor= getContentResolver().query(uri, null, null, null, null);
            if(cursor!=null){
                while (cursor.moveToNext()){
                    String addr=cursor.getString(cursor.getColumnIndexOrThrow("address"));
                    String body=cursor.getString(cursor.getColumnIndexOrThrow("body"));
                    smsList.add("From: "+addr+"\n"+body);
                }
                cursor.close();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        adapter.notifyDataSetChanged();
    }
    public void sendSms(){
        String number=nInp.getText().toString();
        String mess=mInp.getText().toString();
        if(number.isEmpty() || mess.isEmpty()){
            Toast.makeText(this, "All fields are Required.", Toast.LENGTH_SHORT).show();
        }
        try {
            SmsManager smsManager=SmsManager.getDefault();
            smsManager.sendTextMessage(number,null,mess,null,null);
            Toast.makeText(this, "SMS Send.", Toast.LENGTH_SHORT).show();
            mInp.setText("");
        } catch (Exception e) {
            Toast.makeText(this, "Sms sending Failure!", Toast.LENGTH_SHORT).show();
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if(requestCode==SMS_REQUEST_CODE){
            if(grantResults.length>0 && grantResults[0]==PackageManager.PERMISSION_GRANTED){
                readSms();
            }else{
                Toast.makeText(this, "Permission Denied!", Toast.LENGTH_SHORT).show();
            }
        }
    }
}
```

## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/mInp"
        android:layout_width="313dp"
        android:layout_height="83dp"
        android:ems="10"
        android:hint="Enter Message:"
        android:inputType="text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.622"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.228" />

    <EditText
        android:id="@+id/nInp"
        android:layout_width="283dp"
        android:layout_height="39dp"
        android:ems="10"
        android:hint="Enter Number:"
        android:inputType="text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.562"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.134" />

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/mInp"
        app:layout_constraintVertical_bias="0.112" />

    <ListView
        android:id="@+id/lv"
        android:layout_width="365dp"
        android:layout_height="356dp"
        android:scrollbars="vertical"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.957" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
