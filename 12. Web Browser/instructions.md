# Develop an application to show contents of specified URL without using native browser. Also provide facility to navigate to previous and next page as well as clear browsing history.

## 1. Add Permissions

Add the following permission to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

## 2. `activity_main.xml`

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
        android:id="@+id/linearLayout"
        android:layout_width="match_parent"
        android:layout_height="200sp"
        android:gravity="center_horizontal|center_vertical"
        android:orientation="vertical"
        tools:layout_editor_absoluteX="1dp"
        tools:layout_editor_absoluteY="1dp"
        tools:ignore="MissingConstraints">

        <EditText
            android:id="@+id/url"
            android:layout_width="371dp"
            android:layout_height="wrap_content"
            android:layout_marginTop="30sp"
            android:ems="10"
            android:hint="Enter the url:"
            android:inputType="text" />

        <Button
            android:id="@+id/search"
            android:layout_width="262dp"
            android:layout_height="wrap_content"
            android:layout_marginTop="5sp"
            android:layout_marginBottom="7sp"
            android:text="Search" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="horizontal">

            <Button
                android:id="@+id/back"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="5sp"
                android:layout_marginRight="5sp"
                android:layout_weight="1"
                android:text="Back" />

            <Button
                android:id="@+id/forward"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="5sp"
                android:layout_marginRight="5sp"
                android:layout_weight="1"
                android:text="Forward" />

            <Button
                android:id="@+id/clear"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="5sp"
                android:layout_marginRight="5sp"
                android:layout_weight="1"
                android:text="Clear History" />
        </LinearLayout>

    </LinearLayout>

    <WebView
        android:id="@+id/web"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/linearLayout"
        tools:layout_editor_absoluteX="-33dp" />
</androidx.constraintlayout.widget.ConstraintLayout>

```

## 3. `MainActivity.java`

Here’s a clean and easy-to-understand version:

```java
package com.example.mywebbro;

import android.annotation.SuppressLint;
import android.os.Bundle;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.Button;
import android.widget.EditText;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    EditText url;
    Button search,back,forward,clear;
    WebView web;
    @SuppressLint("SetJavaScriptEnabled")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        url=findViewById(R.id.url);
        search=findViewById(R.id.search);
        back=findViewById(R.id.back);
        forward=findViewById(R.id.forward);
        clear=findViewById(R.id.clear);
        web=findViewById(R.id.web);

        web.getSettings().setJavaScriptEnabled(true);
        web.setWebViewClient(new WebViewClient());

        search.setOnClickListener(v->{
            String s=url.getText().toString().trim();
            if(!s.startsWith("https://") || !s.startsWith("http://")){
                s="https://"+s;
            }
            web.loadUrl(s);
        });
        back.setOnClickListener(v->{
            if(web.canGoBack()){
                web.goBack();
            }
        });
        forward.setOnClickListener(v->{
            if(web.canGoForward()){
                web.goForward();
            }
        });
        clear.setOnClickListener(v->{
            web.clearHistory();
        });
    }
}
```
