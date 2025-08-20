## MainActivity.java

```java
package com.example.mydemocrud;

import android.database.Cursor;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    Button iBtn,uBtn,dBtn,aBtn;
    EditText et,uid;
    TextView tv;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        iBtn=findViewById(R.id.iBtn);
        uBtn=findViewById(R.id.uBtn);
        dBtn=findViewById(R.id.dBtn);
        aBtn=findViewById(R.id.aBtn);
        et=findViewById(R.id.et);
        uid=findViewById(R.id.uid);
        tv=findViewById(R.id.tv);


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

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:orientation="vertical">

        <EditText
            android:id="@+id/uid"
            android:layout_width="330dp"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15sp"
            android:ems="10"
            android:hint="ID (Update/Delete)"
            android:inputType="text" />

        <EditText
            android:id="@+id/et"
            android:layout_width="355dp"
            android:layout_height="64dp"
            android:layout_marginBottom="20sp"
            android:ems="10"
            android:hint="Enter name:"
            android:inputType="text" />

        <Button
            android:id="@+id/iBtn"
            android:layout_width="328dp"
            android:layout_height="wrap_content"
            android:text="Insert" />

        <Button
            android:id="@+id/uBtn"
            android:layout_width="323dp"
            android:layout_height="wrap_content"
            android:text="Update" />

        <Button
            android:id="@+id/dBtn"
            android:layout_width="321dp"
            android:layout_height="wrap_content"
            android:text="Delete" />

        <Button
            android:id="@+id/aBtn"
            android:layout_width="316dp"
            android:layout_height="wrap_content"
            android:text="View All Users" />

        <TextView
            android:id="@+id/tv"
            android:layout_width="366dp"
            android:layout_height="wrap_content"
            android:layout_marginTop="10sp"
            android:scrollbars="vertical"
            android:textSize="18sp"
            android:verticalScrollbarPosition="defaultPosition" />
    </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```
