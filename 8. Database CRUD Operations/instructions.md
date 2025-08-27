## DbHelper.java

```java
package com.example.mydemocrud;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

import androidx.annotation.Nullable;

public class DbHelper extends SQLiteOpenHelper {
    private static final String DATABASE_NAME="MyDb.db";
    private static final int DATABASE_VERSION=1;
    private static final String TABLE_NAME="Users";

    public DbHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL("CREATE TABLE "+TABLE_NAME+"("+
                "id INTEGER PRIMARY KEY AUTOINCREMENT,"+
                "name TEXT"+")");
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int i, int i1) {
        db.execSQL("DROP TABLE IF EXISTS "+TABLE_NAME);
        onCreate(db);
    }
    public boolean insertUser(String name){
        SQLiteDatabase db=this.getWritableDatabase();
        ContentValues cv=new ContentValues();
        cv.put("name",name);
        long res=db.insert(TABLE_NAME,null,cv);
        return res!=-1;
    }
    public boolean updateUser(int id,String name){
        SQLiteDatabase db=this.getWritableDatabase();
        ContentValues cv=new ContentValues();
        cv.put("name",name);
        int res=db.update(TABLE_NAME,cv,"id=?",new String[]{String.valueOf(id)});
        return res>0;
    }
    public boolean deleteUser(int id){
        SQLiteDatabase db=this.getWritableDatabase();
        int res=db.delete(TABLE_NAME,"id=?",new String[]{String.valueOf(id)});
        return res>0;
    }
    public Cursor viewAll(){
        SQLiteDatabase db=this.getReadableDatabase();
        return db.rawQuery("SELECT * FROM "+TABLE_NAME,null);
    }
}
```

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

    DbHelper db;

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

        db=new DbHelper(this);

        iBtn.setOnClickListener(v->{
            String name=et.getText().toString().trim();
            if(name.isEmpty()){
                Toast.makeText(this, "Please enter a name!", Toast.LENGTH_SHORT).show();
                return;
            }
            boolean res=db.insertUser(name);
            Toast.makeText(this, res?"Inserted":"Failed", Toast.LENGTH_SHORT).show();
            if(res){
                et.setText("");
            }
        });
        uBtn.setOnClickListener(v->{
            String name=et.getText().toString().trim();
            String id=uid.getText().toString().trim();
            if(name.isEmpty() || id.isEmpty()){
                Toast.makeText(this, "Please enter all details!", Toast.LENGTH_SHORT).show();
                return;
            }
            boolean res=db.updateUser(Integer.parseInt(id),name);
            Toast.makeText(this, res?"Updated":"Failed", Toast.LENGTH_SHORT).show();
            if(res){
                et.setText("");
                uid.setText("");
            }
        });
        dBtn.setOnClickListener(v->{
            String id=uid.getText().toString().trim();
            if(id.isEmpty()){
                Toast.makeText(this, "Please enter id!", Toast.LENGTH_SHORT).show();
                return;
            }
            boolean res=db.deleteUser(Integer.parseInt(id));
            Toast.makeText(this, res?"Deleted":"Failed", Toast.LENGTH_SHORT).show();
            if(res){
                et.setText("");
                uid.setText("");
            }
        });
        aBtn.setOnClickListener(v->{
            Cursor cursor=db.viewAll();
            StringBuilder sb=new StringBuilder();
            while (cursor.moveToNext()){
                sb.append("ID: ").append(cursor.getInt(0)).append("\n");
                sb.append("Name: ").append(cursor.getString(1)).append("\n\n");
            }
            tv.setText(sb.toString());
        });
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
