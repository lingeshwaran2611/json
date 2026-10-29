Develop an Android application to fetch and parse JSON data from a web
← Back to Index
12. Develop an Android application to fetch and parse JSON data from a web
PROJECTNAME: JSONDEMO
Add Internet Permission.
AndroidManifest.xml
Add following above: <application> tag
<uses-permission android:name="android.permission.INTERNET"/>
activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
 xmlns:android="http://schemas.android.com/apk/res/android"
 android:layout_width="match_parent"
 android:layout_height="match_parent"
 android:orientation="vertical"
 android:padding="20dp">
 <Button
 android:id="@+id/btnFetch"
 android:layout_width="match_parent"
 android:layout_height="wrap_content"
 android:text="Fetch JSON Data" />
 <TextView
 android:id="@+id/txtResult"
 android:layout_width="match_parent"
 android:layout_height="wrap_content"
 android:layout_marginTop="20dp"
 android:textSize="18sp"
 android:textColor="#000000" />
</LinearLayout>
MainActivity.java
package com.example.jsondemo;
import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import org.json.JSONObject;
import androidx.appcompat.app.AppCompatActivity;
public class MainActivity extends AppCompatActivity {
 Button btnFetch;
 TextView txtResult;
 @Override
 protected void onCreate(Bundle savedInstanceState) {
 super.onCreate(savedInstanceState);
 setContentView(R.layout.activity_main);
 btnFetch = findViewById(R.id.btnFetch);
 txtResult = findViewById(R.id.txtResult);
 btnFetch.setOnClickListener(v -> {
 fetchData();
 });
 }
 private void fetchData() {
 new Thread(() -> {
 try {
 // Web Service URL
 URL url = new
URL("https://jsonplaceholder.typicode.com/posts/1");
 HttpURLConnection connection =
 (HttpURLConnection) url.openConnection();
 connection.setRequestMethod("GET");
 BufferedReader reader = new BufferedReader(new
InputStreamReader(
 connection.getInputStream()
 )
);
StringBuilder result = new StringBuilder();
String line;
while ((line = reader.readLine()) != null) {
 result.append(line);
}
reader.close();
// JSON Parsing
JSONObject jsonObject =
 new JSONObject(result.toString());
String title =
 jsonObject.getString("title");
 String body =
 jsonObject.getString("body");
 // Display Data
 runOnUiThread(() -> {
 txtResult.setText(
 "Title: " + title +
 "\n\nBody: " + body
 );
 });
 connection.disconnect();
} catch (Exception e) {
 e.printStackTrace();
 runOnUiThread(() -> {
 txtResult.setText(
 "Error: " + e.getMessage()
 );
 });
}
 }).start();
 }
}
 https://jsonplaceholder.typicode.com/posts/1
