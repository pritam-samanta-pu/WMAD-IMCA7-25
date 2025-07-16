# Dynamic Generation of Widgets

## `activity_main.xml`

Basic layout with just a Button and a EditText.

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
        android:id="@+id/inp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint='Enter a number:'
        android:inputType="numberDecimal"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.304" />

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Submit"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/inp"
        app:layout_constraintVertical_bias="0.128" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

## `MainActivity.java`

Here’s a clean and easy-to-understand version:

```java
package com.example.tempgen;

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

public class MainActivity extends AppCompatActivity {

    private EditText inp;
    private Button btn;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        //coding start from here

        inp=findViewById(R.id.inp);
        btn=findViewById(R.id.btn);
        btn.setOnClickListener(v->{
            String s=inp.getText().toString().trim();
            if(s.isEmpty()){
                Toast.makeText(this, "Input Field is Empty!", Toast.LENGTH_SHORT).show();
                return;
            }
            int count=Integer.parseInt(s);
            Intent intent=new Intent(MainActivity.this,SecondActivity.class);
            intent.putExtra("count",count);
            startActivity(intent);
        });
    }
}
```

## `activity_second.xml`

Basic layout with just a TextView and a ListView.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SecondActivity">

    <ListView
        android:id="@+id/lv"
        android:layout_width="310dp"
        android:layout_height="438dp"
        android:scrollbars="vertical"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.495"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.61" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="119dp"
        android:layout_height="50dp"
        android:text="List View:"
        android:textSize="24sp"
        app:layout_constraintBottom_toTopOf="@+id/lv"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.534"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.798" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

## `SecondActivity.java`

Here’s a clean and easy-to-understand version:

```java
package com.example.tempgen;

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

public class SecondActivity extends AppCompatActivity {
    private ListView lv;
    ArrayList<String> arrayList;
    ArrayAdapter<String> adapter;
    @SuppressLint("MissingInflatedId")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_second);

        lv=findViewById(R.id.iv);
        int count=getIntent().getIntExtra("count",0);
        arrayList=new ArrayList<>();
        for (int i=1;i<=count;i++){
            arrayList.add("Item "+i);
        }
        adapter=new ArrayAdapter<>(this, android.R.layout.simple_list_item_1,arrayList);
        lv.setAdapter(adapter);
    }
}
```
