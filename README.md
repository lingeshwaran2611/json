# 12. Develop an Android Application to Fetch and Parse JSON Data from the Web

## Project Name: `JSONDEMO`

## Aim
To develop an Android application that fetches JSON data from a web service using HTTP GET and parses the JSON response to display the title and body in the application.

## Technologies Used
- **Programming Language:** Java
- **IDE:** Android Studio
- **Data Format:** JSON (JavaScript Object Notation)
- **Networking:** `HttpURLConnection`
- **JSON Parsing:** `JSONObject`
- **Web Service:** JSONPlaceholder API

## Project Structure

```text
JSONDEMO/
├── app/
│   └── src/
│       └── main/
│           ├── java/
│           │   └── com/
│           │       └── example/
│           │           └── jsondemo/
│           │               └── MainActivity.java
│           ├── res/
│           │   └── layout/
│           │       └── activity_main.xml
│           └── AndroidManifest.xml
└── README.md
```

---

## 1. AndroidManifest.xml

**Location:** `app/src/main/AndroidManifest.xml`

Add the Internet permission above the `<application>` tag to allow the application to access the web service.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission
        android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:label="JSONDEMO"
        android:supportsRtl="true"
        android:theme="@style/Theme.JSONDEMO">

        <activity
            android:name=".MainActivity"
            android:exported="true">

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

        </activity>

    </application>

</manifest>
```

**Note:** If your Android Studio project uses a different theme name, retain the existing theme reference.

---

## 2. activity_main.xml

**Location:** `app/src/main/res/layout/activity_main.xml`

This XML file defines a button to fetch JSON data and a TextView to display the parsed response.

```xml
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
```

---

## 3. MainActivity.java

**Location:** `app/src/main/java/com/example/jsondemo/MainActivity.java`

This Java class connects to the web service, fetches the JSON response, parses the `title` and `body` fields, and displays the data in the TextView.

```java
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
                URL url = new URL(
                    "https://jsonplaceholder.typicode.com/posts/1"
                );

                HttpURLConnection connection =
                    (HttpURLConnection) url.openConnection();

                connection.setRequestMethod("GET");

                BufferedReader reader = new BufferedReader(
                    new InputStreamReader(
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

                String title = jsonObject.getString("title");
                String body = jsonObject.getString("body");

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
```

---

## 4. Web Service URL

The application fetches data from the following API endpoint:

[https://jsonplaceholder.typicode.com/posts/1](https://jsonplaceholder.typicode.com/posts/1)

This endpoint returns a JSON object containing a post ID, user ID, title, and body.

### Sample JSON Response

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
}
```

---

## 5. How to Run the Application

1. Open Android Studio.
2. Create a new Android project named `JSONDEMO`.
3. Select **Java** as the programming language.
4. Set the package name to `com.example.jsondemo`.
5. Add the Internet permission in `AndroidManifest.xml`.
6. Replace the code in `activity_main.xml` with the provided XML code.
7. Replace the code in `MainActivity.java` with the provided Java code.
8. Ensure the Android theme reference matches your project.
9. Run the application on an emulator or Android device with an Internet connection.
10. Click the **Fetch JSON Data** button to retrieve and display the JSON response.

---

## 6. Expected Output

When the application starts, it displays a button:

```text
+----------------------------------+
|                                  |
|       Fetch JSON Data            |
|                                  |
+----------------------------------+
```

After clicking the button, the application displays the fetched data:

```text
Title: sunt aut facere repellat
provident occaecati excepturi
optio reprehenderit

Body: quia et suscipit
suscipit recusandae consequuntur
expedita et cum
reprehenderit molestiae ut ut quas
totam nostrum rerum est autem
sunt rem eveniet architecto
```

The displayed title and body are obtained from the web service response.

---

## 7. Result

Thus, an Android application was developed successfully to fetch JSON data from a web service using HTTP GET. The JSON response was parsed using `JSONObject`, and the title and body were displayed in the TextView.

## 8. Key Concepts

- **Internet Permission:** Allows the application to access the Internet.
- **HTTP GET:** Requests data from the web service.
- **HttpURLConnection:** Establishes the HTTP connection.
- **BufferedReader:** Reads the response from the server.
- **StringBuilder:** Collects the response text.
- **JSONObject:** Parses the JSON response.
- **getString():** Retrieves the title and body fields.
- **runOnUiThread():** Updates the user interface from the background thread.
- **Thread:** Performs network operations outside the main UI thread.

---

## 9. Important Notes

- An active Internet connection is required.
- Network operations must not run on the main UI thread.
- The API endpoint must be reachable for data retrieval to succeed.
- If the request fails, the application displays an error message.
- For production applications, consider adding HTTP response-code checks, connection timeouts, and proper resource cleanup.
