# Shared Preferences

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
        android:id="@+id/rBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Red"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.253"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.499" />

    <Button
        android:id="@+id/bBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Blue"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.705"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.499" />

    <Button
        android:id="@+id/dBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Default"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.644" />
</androidx.constraintlayout.widget.ConstraintLayout>

```

## MainActivity.java

```java
package com.example.mysharedpref; // Your Project name in smallcase

import android.content.SharedPreferences;
import android.graphics.Color;
import android.os.Bundle;
import android.widget.Button;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.constraintlayout.widget.ConstraintLayout;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    Button rBtn,bBtn,dBtn;
    ConstraintLayout main;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        rBtn=findViewById(R.id.rBtn);
        bBtn=findViewById(R.id.bBtn);
        dBtn=findViewById(R.id.dBtn);
        main=findViewById(R.id.main);
        fetchAndSave();

        rBtn.setOnClickListener(v->saveAndSet(Color.RED));
        bBtn.setOnClickListener(v->saveAndSet(Color.BLUE));
        dBtn.setOnClickListener(v->saveAndSet(Color.TRANSPARENT));
    }
    public void saveAndSet(int colorId){
        SharedPreferences pref=getSharedPreferences("MyPrefs",MODE_PRIVATE);
        SharedPreferences.Editor editor=pref.edit();
        editor.putInt("BGCOLOR",colorId);
        editor.apply();
        main.setBackgroundColor(colorId);
        Toast.makeText(this, "Background Color Saved.", Toast.LENGTH_SHORT).show();
    }
    public void fetchAndSave(){
        SharedPreferences pref=getSharedPreferences("MyPrefs",MODE_PRIVATE);
        main.setBackgroundColor(pref.getInt("BGCOLOR", Color.TRANSPARENT));
    }
}
```
