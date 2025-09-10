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
        android:orientation="vertical"
        android:padding="15sp">

        <Spinner
            android:id="@+id/spinner"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />

        <ImageView
            android:id="@+id/img"
            android:layout_width="match_parent"
            android:layout_height="221dp"
            app:srcCompat="@android:mipmap/sym_def_app_icon" />
    </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

## MainActivity.java

```java
package com.example.myapplicationani;

import android.os.Bundle;
import android.view.View;
import android.view.animation.Animation;
import android.view.animation.AnimationUtils;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ImageView;
import android.widget.Spinner;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Arrays;

public class MainActivity extends AppCompatActivity {
    Spinner spinner;
    ImageView img;
    ArrayList<String> list=new ArrayList<>(Arrays.asList("Fade in","Rotate","Slide_in_left"));
    ArrayAdapter<String> adapter;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        spinner=findViewById(R.id.spinner);
        img=findViewById(R.id.img);

        adapter=new ArrayAdapter<>(this, android.R.layout.simple_list_item_1,list);
        adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        spinner.setAdapter(adapter);

        spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> adapterView, View view, int pos, long l) {
                Animation animation=null;
                switch (pos){
                    case 0:
                        animation= AnimationUtils.loadAnimation(getApplicationContext(),R.anim.fade_in);
                        break;
                    case 1:
                        animation= AnimationUtils.loadAnimation(getApplicationContext(),R.anim.rotate);
                        break;
                    case 2:
                        animation= AnimationUtils.loadAnimation(getApplicationContext(),R.anim.slide_in_left);
                        break;
                }
                if (animation!=null){
                    img.startAnimation(animation);

                }
            }

            @Override
            public void onNothingSelected(AdapterView<?> adapterView) {

            }
        });
    }
}
```

# Add Animations

Create a folder under res `res/anim` and create these files:

## fade_in.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromAlpha="0.0"
    android:toAlpha="1.0"
    android:duration="1000"
    />
```

## rotate.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<rotate xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromDegrees="0"
    android:toDegrees="365"
    android:pivotX="50%"
    android:pivotY="50%"
    android:duration="1000"
    />
```

## slide_in_left

```xml
<?xml version="1.0" encoding="utf-8"?>
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromXDelta="-100%"
    android:toXDelta="0%"
    android:duration="1000"
    />
```
