# File I/O

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

    <Button
        android:id="@+id/aBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="About ->"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.524"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.701" />

    <Button
        android:id="@+id/saveBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="#24A62A"
        android:text="Save"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.5" />

    <EditText
        android:id="@+id/inp"
        android:layout_width="290dp"
        android:layout_height="53dp"
        android:ems="10"
        android:hint="Enter a text:"
        android:inputType="text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.354" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

## second_activity.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SecondActivity">

    <Button
        android:id="@+id/fetchBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Fetch"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.5" />

    <TextView
        android:id="@+id/tv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="serif-monospace"
        android:textSize="20sp"
        app:layout_constraintBottom_toTopOf="@+id/fetchBtn"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.678" />

    <Button
        android:id="@+id/hBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Home ->"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.512"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.73" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

## MainActivity.java

```java
package com.example.myfileioapp; //Your Project name in smallcase

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.io.FileOutputStream;
import java.io.IOException;

public class MainActivity extends AppCompatActivity {
    EditText inp;
    Button saveBtn,aBtn;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        inp=findViewById(R.id.inp);
        saveBtn=findViewById(R.id.saveBtn);
        aBtn=findViewById(R.id.aBtn);

        aBtn.setOnClickListener(v->{
            Intent intent=new Intent(this,SecondActivity.class);
            startActivity(intent);
        });
        saveBtn.setOnClickListener(v->{
            String FILE_NAME="MyFile.txt";
            String s=inp.getText().toString().trim();

            try{
                FileOutputStream fos=openFileOutput(FILE_NAME,MODE_PRIVATE);
                fos.write(s.getBytes());
                fos.close();
                Toast.makeText(this, "Data Saved.", Toast.LENGTH_SHORT).show();
            }catch (Exception e){
                e.printStackTrace();
            }
        });
    }
}
```

## SecondActivity.java

```java
package com.example.myfileioapp; // Your Project name in smallcase

import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.io.FileInputStream;

public class SecondActivity extends AppCompatActivity {
    TextView tv;
    Button fetchBtn,hBtn;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_second);

        tv=findViewById(R.id.tv);
        fetchBtn=findViewById(R.id.fetchBtn);
        hBtn=findViewById(R.id.hBtn);

        hBtn.setOnClickListener(v->finish());
        fetchBtn.setOnClickListener(v->{
            String FILE_NAME="MyFile.txt";
            int c;
            StringBuilder sb=new StringBuilder();
            try {
                FileInputStream fis=openFileInput(FILE_NAME);
                while((c=fis.read())!=-1){
                    sb.append((char)c);
                }
                fis.close();
                String s=sb.toString();
                tv.setText(s);
                Toast.makeText(this, "Data Fetched.", Toast.LENGTH_SHORT).show();
            }catch (Exception e){
                e.printStackTrace();
            }
        });
    }
}
```
