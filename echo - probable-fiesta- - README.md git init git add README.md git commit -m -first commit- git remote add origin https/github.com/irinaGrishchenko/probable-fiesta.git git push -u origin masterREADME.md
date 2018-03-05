# probable-fiesta
package com.example.irina.lesson34_39_sqlite;

import android.content.ContentValues;
import android.content.Context;
import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class Activity1 extends AppCompatActivity {

    final String LOG_TAG = "myLogs";
    Button btnAdd, btnRead, btnClear, btnNext;
    EditText etName, etEmail;

    DataBaseHelper helper;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_1);

        btnAdd = findViewById(R.id.btnAdd);
        btnRead = findViewById(R.id.btnRead);
        btnClear = findViewById(R.id.btnClear);
        btnNext = findViewById(R.id.btnNext);

        helper = new DataBaseHelper(this);
    }

    public void OnClick(View view) {
        ContentValues cv = new ContentValues();

            String name = etName.getText().toString();
            String email = etEmail.getText().toString();

            SQLiteDatabase db = helper.getWritableDatabase();

            switch (view.getId()) {
                case R.id.btnAdd:
                    Log.d(LOG_TAG, "Insert in mutable");

                    cv.put("name", name);
                    cv.put("email", email);

                    long rowID = db.insert("mytable", null, cv);
                    Log.d(LOG_TAG, "row inserted, id = " + rowID);
                    break;
                case R.id.btnRead:
                    Log.d(LOG_TAG, "Rows in mytable:");
                    Cursor c = db.query("mytable", null, null, null, null, null, null);
                    if (c.moveToFirst()) {
                        int idColIndex = c.getColumnIndex("id");
                        int nameColIndex = c.getColumnIndex("name");
                        int emailColIndex = c.getColumnIndex("email");

                        do {
                            Log.d(LOG_TAG,
                                    "ID = " + c.getInt(idColIndex) +
                                            ", name =  " + c.getInt(nameColIndex) +
                                            ", email = " + c.getInt(emailColIndex));
                        } while (c.moveToNext());
                    } else
                        Log.d(LOG_TAG, "0 rows");
                    c.close();
                    break;
                case R.id.btnClear:
                    Log.d(LOG_TAG, "Clear mytable");
                    int clearCount = db.delete("mytable", null, null);
                    Log.d(LOG_TAG, "deleted rows count = " + clearCount);
                    break;
               /* case R.id.btnNext:
                    Intent intent;
                    startActivity(new Intent(this, Activity2.class));*/
            }
            helper.close();


    }

    class DataBaseHelper extends SQLiteOpenHelper {
        public DataBaseHelper(Context context) {
            super(context, "myDB.db", null, 1);
        }

        @Override
        public void onCreate(SQLiteDatabase db) {
            Log.d(LOG_TAG, "onCreate database");
            db.execSQL("create table mytable ("
            + "id integer primary key autoincrement,"
            + "name text,"
            + "email text" + ");");

        }

        @Override
        public void onUpgrade(SQLiteDatabase sqLiteDatabase, int i, int i1) {

        }
    }
    }

