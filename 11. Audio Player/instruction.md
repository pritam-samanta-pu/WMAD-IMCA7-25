## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity2">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:orientation="vertical">

        <Button
            android:id="@+id/play"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginBottom="30sp"
            android:text="Play" />

        <Button
            android:id="@+id/pause"
            android:layout_width="154dp"
            android:layout_height="wrap_content"
            android:layout_marginBottom="30sp"
            android:text="Pause"
            tools:layout_editor_absoluteX="115dp"
            tools:layout_editor_absoluteY="167dp" />

        <Button
            android:id="@+id/stop"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginBottom="30sp"
            android:text="Stop" />
    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

## MainActivity.java

```java
package com.example.mydemo;

import android.media.MediaPlayer;
import android.os.Bundle;
import android.widget.Button;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity2 extends AppCompatActivity {
    Button play,pause,stop;
    MediaPlayer mediaPlayer;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main2);

        play=findViewById(R.id.play);
        pause=findViewById(R.id.pause);
        stop=findViewById(R.id.stop);

        mediaPlayer=MediaPlayer.create(this,R.raw.song);

        play.setOnClickListener(v->{
            if(!mediaPlayer.isPlaying()){
                mediaPlayer.start();
            }
        });
        pause.setOnClickListener(v->{
            if(mediaPlayer.isPlaying()){
                mediaPlayer.pause();
            }
        });
        stop.setOnClickListener(v->{
            if(mediaPlayer.isPlaying()){
                mediaPlayer.stop();
                mediaPlayer=MediaPlayer.create(this,R.raw.song);
            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if(mediaPlayer!=null){
            mediaPlayer.release();
            mediaPlayer=null;
        }
    }
}
```

# Add A Music into your project

Create a folder under res, `res/raw` and add your music file like `song.mp3`
