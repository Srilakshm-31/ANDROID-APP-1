
JAVA AND XML CODE:
MAIN ACTIVITY	
JAVA CODE
package com.example.medcotrack;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.os.Handler;
import android.widget.ImageView;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
public class MainActivity extends AppCompatActivity {
    TextView tv1, tv2;
    ImageView im1;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        tv1 = findViewById(R.id.textView2);
        tv2 = findViewById(R.id.textView3);
        im1 = findViewById(R.id.imageView2);
        new Handler().postDelayed(() -> {
            SharedPreferences prefs = getSharedPreferences("UserPrefs", MODE_PRIVATE);
boolean isLoggedIn = prefs.getBoolean("isLoggedIn", false);
            if (isLoggedIn) {
                startActivity(new Intent(MainActivity.this, MainActivity4.class)); // Go to dashboard
            } else {
                startActivity(new Intent(MainActivity.this, MainActivity2.class)); // Go to login
}
            finish();
    }, 3000); // 3-second splash (adjust as needed)
}
}
XML CODE
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/imageView2"
        android:layout_width="249dp"
        android:layout_height="196dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.496"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.42"
        app:srcCompat="@drawable/medcotackk" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="214dp"
        android:layout_height="36dp"
        android:text="MedcoTrack"
        android:textAlignment="center"
        android:textSize="24sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.624" />

    <TextView
        android:id="@+id/textView3"
        android:layout_width="377dp"
        android:layout_height="33dp"
        android:text="Stay Healthy, Stay Remainded"
        android:textAlignment="center"
        android:textSize="20sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.496"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.684" />
</androidx.constraintlayout.widget.ConstraintLayout>
          
 
MAIN ACTIVITY 2
JAVA CODE
package com.example.medcotrack;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.text.TextUtils;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
import com.google.android.gms.auth.api.signin.GoogleSignIn;
import com.google.android.gms.auth.api.signin.GoogleSignInAccount;
import com.google.android.gms.auth.api.signin.GoogleSignInClient;
import com.google.android.gms.auth.api.signin.GoogleSignInOptions;
import com.google.android.gms.common.SignInButton;
import com.google.android.gms.common.api.ApiException;
import com.google.android.gms.tasks.Task;
public class MainActivity2 extends AppCompatActivity {
EditText etUsername, etPassword;
    Button btnLogin;
    SignInButton btnGoogleSignIn;
    GoogleSignInClient mGoogleSignInClient;
    public static final String PREFS_NAME = "MyPrefs";
    public static final String KEY_LOGGED_IN = "isLoggedIn";
    public static final String KEY_NAME = "name";
    public static final String KEY_AGE = "age";
    public static final String KEY_GENDER = "gender";
    private static final int RC_SIGN_IN = 101;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // ✅ Already logged in user → Go to MainActivity4
        SharedPreferences prefs = getSharedPreferences(PREFS_NAME,
MODE_PRIVATE);
        if (prefs.getBoolean(KEY_LOGGED_IN, false)) {            startActivity(new Intent(MainActivity2.this, MainActivity4.class));
            finish();
            return;
        }
        setContentView(R.layout.activity_main2);
etUsername = findViewById(R.id.etUsername);
        etPassword = findViewById(R.id.etPassword);
        btnLogin = findViewById(R.id.btnLogin);
        btnGoogleSignIn = findViewById(R.id.btnGoogleSignIn);
        // ✅ Email/Password Login
        btnLogin.setOnClickListener(v -> {
            String user = etUsername.getText().toString().trim();
            String pass = etPassword.getText().toString().trim();
            if (TextUtils.isEmpty(user) || TextUtils.isEmpty(pass)) {
                Toast.makeText(MainActivity2.this, "Please enter username and password", Toast.LENGTH_SHORT).show();
            } else {
  // Save login + dummy name/age/gender
  SharedPreferences.Editor editor = getSharedPreferences(PREFS_NAME, MODE_PRIVATE).edit();      editor.putBoolean(KEY_LOGGED_IN, true);
                editor.putString(KEY_NAME, user); // dummy name
                editor.putString(KEY_AGE, "21");  // dummy age
                editor.putString(KEY_GENDER, "Female"); // dummy gender
                editor.apply();
                Toast.makeText(MainActivity2.this, "Login successful", Toast.LENGTH_SHORT).show();
                startActivity(new Intent(MainActivity2.this,
MainActivity3.class)); // ⬅️ First time profile details
                finish();
            }
        });
  // ✅ Google Sign-In Setup
        GoogleSignInOptions gso = new
GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                .requestEmail()
                .build();
     mGoogleSignInClient = GoogleSignIn.getClient(this, gso);
        btnGoogleSignIn.setOnClickListener(v -> {
            Intent signInIntent = mGoogleSignInClient.getSignInIntent();
            startActivityForResult(signInIntent, RC_SIGN_IN);
});
    }
    @Overrid
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == RC_SIGN_IN) {
            Task<GoogleSignInAccount> task = GoogleSignIn.getSignedInAccountFromIntent(data);
            try {
                GoogleSignInAccount account = task.getResult(ApiException.class);
                if (account != null) {
    SharedPreferences.Editor editor = getSharedPreferences(PREFS_NAME, MODE_PRIVATE).edit();
                    editor.putBoolean(KEY_LOGGED_IN, true);
                    editor.putString(KEY_NAME, account.getDisplayName());
                    editor.putString(KEY_AGE, "21"); // default value, can update in MainActivity3
                    editor.putString(KEY_GENDER, "Female"); // default
   editor.apply();
                    Toast.makeText(this, "Signed in as " + account.getDisplayName(), Toast.LENGTH_SHORT).show();
                    startActivity(new Intent(MainActivity2.this, MainActivity3.class));
 finish();                }
            } catch (ApiException e) {
                Toast.makeText(this, "Sign-in failed: " + e.getMessage(), Toast.LENGTH_SHORT).show();
            }
        }
    }
}

XML CODE:
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
  xmlns:tools="http://schemas.android.com/tools"    android:id="@+id/main"
    android:layout_width="match_parent    android:layout_height="match_parent"
    android:padding="24dp"
    android:background="#ffffff"
    tools:context=".MainActivity2">
    <TextView
        android:id="@+id/appTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Welcome to MedCoTrack"
        android:textColor="#000000"
        android:textSize="22sp"
    android:textStyle="bold"
       app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintVertical_bias="0.303" />
    <EditText
        android:id="@+id/etUsername"
        android:layout_width="0dp"
 android:layout_height="wrap_content"        android:layout_marginTop="24dp"
        android:background="@android:drawable/edit_text"
        android:hint="Username"
        android:inputType="textPersonName"
 android:padding="12dp        app:layout_constraintBottom_toBottomOf="parent"
 app:layout_constraintEnd_toEndOf="parent        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/appTitle"
        app:layout_constraintVertical_bias="0.0" />
    <EditText
        android:id="@+id/etPassword"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:background="@android:drawable/edit_text"
        android:hint="Password"
        android:inputType="textPassword"
        android:padding="12dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/etUsername"
        app:layout_constraintVertical_bias="0.003" />
    <Button
        android:id="@+id/btnLogin"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:text="Sign In"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"      app:layout_constraintHorizontal_bias="0.0"      app:layout_constraintStart_toStartOf="parent"
   app:layout_constraintTop_toBottomOf="@id/etPassword"
        app:layout_constraintVertical_bias="0.0" />
    <TextView
android:id="@+id/tvOr"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="30dp"
   android:text="Or sign in with"
        android:textColor="#888888"
    app:layout_constraintBottom_toBottomOf="parent"     app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
   app:layout_constraintTop_toBottomOf="@id/btnLogin"
        app:layout_constraintVertical_bias="0.0" />
    <LinearLayout
   android:id="@+id/socialLoginLayout"     android:layout_width="0dp"
android:layout_height="wrap_content"        android:layout_marginTop="16dp"
        android:gravity="center"
      android:orientation="horizontal"        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
   app:layout_constraintTop_toBottomOf="@id/tvOr"
app:layout_constraintVertical_bias="0.0">        <com.google.android.gms.common.SignInButton
 android:id="@+id/btnGoogleSignIn"
android:layout_width="wrap_content"            android:layout_height="wrap_content" />
    </LinearLayout>
    <ImageView
        android:id="@+id/imageView2"
        android:layout_width="114dp"
        android:layout_height="136dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.067"
        app:srcCompat="@drawable/medcotackk" />
</androidx.constraintlayout.widget.ConstraintLayout>
 
MAIN ACTIVITY 3
JAVA CODE:
package com.example.medcotrack;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
public class MainActivity3 extends AppCompatActivity {
    EditText nameInput, ageInput, genderInput;
    Button nextButton;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main3);
        nameInput = findViewById(R.id.nameInput);
        ageInput = findViewById(R.id.ageInput);
        genderInput = findViewById(R.id.genderInput);
        nextButton = findViewById(R.id.nextButton);
        genderInput.setOnClickListener(v -> showGenderDialog());
        nextButton.setOnClickListener(v -> {
            String name = nameInput.getText().toString().trim();
            String age = ageInput.getText().toString().trim();
            String gender = genderInput.getText().toString().trim();
            if (name.isEmpty() || age.isEmpty() || gender.isEmpty()) {
                Toast.makeText(this, "Please fill all fields", Toast.LENGTH_SHORT).show();
                return;
    }
            // ✅ Save to SharedPreferences
            SharedPreferences preferences getSharedPreferences("MedcoPrefs", MODE_PRIVATE);
   SharedPreferences.Editor editor = preferences.edit();            editor.putString("user_name", name);
   editor.putString("user_age", age);
            editor.putString("user_gender", gender);
            editor.apply();
            // Move to MainActivity4
            Intent intent = new Intent(MainActivity3.this, MainActivity4.class);
     startActivity(intent);
        });
    }
    private void showGenderDialog() {
        final String[] genderOptions = {"Male", "Female", "Other"};
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("Select Gender");
        builder.setItems(genderOptions, (dialog, which) -> {
            genderInput.setText(genderOptions[which]);
        });
        builder.show();
    }
}

XML CODE:
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/white"
    android:padding="24dp">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:gravity="center_horizontal">
  <ImageView
            android:id="@+id/appLogo"
            android:layout_width="100dp"
  android:layout_height="100dp" android:layout_marginTop="40dp"            android:layout_marginBottom="16dp"
            android:contentDescription="App Logo"
            android:src="@drawable/medcotackk" />
        <TextView
            android:id="@+id/appTitle"
  android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="MedcoTrack"
            android:textSize="24sp"
            android:textStyle="bold"
            android:textColor="@color/black"
            android:layout_marginBottom="32dp" />
        <EditText
            android:id="@+id/nameInput"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Enter your Name"
            android:inputType="textPersonName"
            android:padding="12dp"
            android:background="@drawable/edittext_bg"
            android:layout_marginBottom="16dp" />
        <EditText
            android:id="@+id/ageInput"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Enter your Age"
   android:inputType="number"
            android:padding="12dp"
            android:background="@drawable/edittext_bg"
            android:layout_marginBottom="16dp" />
        <EditText
            android:id="@+id/genderInput"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Select Gender"
            android:focusable="false"
            android:clickable="true"
 android:padding="12dp"
            android:background="@drawable/edittext_bg"
            android:layout_marginBottom="24dp" />
        <Button
            android:id="@+id/nextButton"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Next"
 android:textAllCaps="false"
            android:textSize="18sp"            android:backgroundTint="@color/black"
            android:textColor="@android:color/white"
            android:padding="12dp" />
    </LinearLayout>
</ScrollView>
 
MAIN ACTIVITY 4
JAVA CODE 
package com.example.medcotrack;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.ProgressBar;
import android.widget.TextView;
import androidx.appcompat.app.AlertDialog
import androidx.appcompat.app.AppCompatActivity;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;
import java.util.Calendar;
public class MainActivity4 extends AppCompatActivity {
    ProgressBar testProgressBar;
    TextView testCount, testStatus;
    Button addTestButton;
    ImageView profileIcon;
    SharedPreferences sharedPreferences;
    private static final String PREFS_NAME = "TestPrefs";
    private static final String TEST_LIST_KEY = "SelectedTests";
    private static final String USER_PREFS = "UserPrefs";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main4);
        testProgressBar = findViewById(R.id.testProgressBar);
    testCount = findViewById(R.id.testCount);
        testStatus = findViewById(R.id.testStatus);
        addTestButton = findViewById(R.id.addTestButton);
        profileIcon = findViewById(R.id.profileIcon);
        sharedPreferences = getSharedPreferences(PREFS_NAME, MODE_PRIVATE);
        loadTestData();
        addTestButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity4.this, MainActivity5.class);
      startActivity(intent);
            }
        });
        profileIcon.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
      SharedPreferences userPrefs = getSharedPreferences(USER_PREFS, MODE_PRIVATE);
                String name = userPrefs.getString("name", "Name not set");
   String age = userPrefs.getString("age", "Age not set");
                String gender = userPrefs.getString("gender", "Gender not set");
                String message = "Name: " + name + "\nAge: " + age + "\nGender: " + gender;
                AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity4.this);
   builder.setTitle("User Details");
                builder.setMessage(message);
                builder.setPositiveButton("OK", null);
                builder.show();
}
        });
    }
    private void loadTestData() {
        sharedPreferences = getSharedPreferences(PREFS_NAME, MODE_PRIVATE);
  String json = sharedPreferences.getString(TEST_LIST_KEY, null);        int count = 0;
        boolean testToday = false;
        if (json != null) {
            try {
                JSONArray jsonArray = new JSONArray(json);
                count = jsonArray.length();
                for (int i = 0; i < jsonArray.length(); i++) {
                    JSONObject test = jsonArray.getJSONObject(i);
    String date = test.getString("date");
                    if (isToday(date)) {
                        testToday = true;
break;                    }
                }
            } catch (JSONException e) {
    e.printStackTrace();
            }
        }
        testCount.setText(String.valueOf(count));
        testProgressBar.setProgress(count);
        testProgressBar.setMax(10); // Assuming a max of 10 tests
        testStatus.setText(testToday ? "Test Today" : "No Test Today");
    }
    private boolean isToday(String dateStr) {
        String[] parts = dateStr.split("-");
      if (parts.length != 3) return false;
        int year = Integer.parseInt(parts[0]);
        int month = Integer.parseInt(parts[1]) - 1;
        int day = Integer.parseInt(parts[2]);
     Calendar calendar = Calendar.getInstance();
        return calendar.get(Calendar.YEAR) == year &&
                calendar.get(Calendar.MONTH) == month &&
 calendar.get(Calendar.DAY_OF_MONTH) == day;

    }
    @Override
    protected void onResume() {
        super.onResume();
        loadTestData();
    }
}
XML CODE
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main4"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFF4F4"
    tools:context=".MainActivity4">
   <!-- App Icon and Name -->
    <LinearLayout
    android:id="@+id/headerLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="16dp"
        android:orientation="horizontal"
        android:gravity="center_vertical">
        <ImageView
            android:id="@+id/appIcon"
            android:layout_width="40dp"
            android:layout_height="40dp"
            android:contentDescription="App Icon"
            android:src="@drawable/medcotackk" />
        <TextView
      android:id="@+id/appName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="MedcoTrack"
            android:textSize="20sp"
            android:textStyle="bold"
            android:paddingStart="10dp" />
    </LinearLayout>
    <!-- Profile Icon -->
    <ImageView
        android:id="@+id/profileIcon"
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:layout_alignParentTop="true"        android:layout_alignParentEnd="true"        android:layout_margin="16dp"
        android:background="@drawable/profile"
        android:contentDescription="Profile"
        android:padding="4dp"
    android:src="@drawable/profile" />
    <!-- ProgressBar and Count -->
    <LinearLayout
        android:id="@+id/progressLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:layout_centerInParent="true"
        android:gravity="center"
        android:padding="16dp">
        <ProgressBar
android:id="@+id/testProgressBar"
            style="@android:style/Widget.ProgressBar.Horizontal"
            android:layout_width="150dp"
 android:layout_height="150dp"
            android:layout_marginBottom="16dp"
            android:indeterminateOnly="false"
            android:max="10"
            android:progress="0"
            android:progressDrawable="@drawable/progressbar"
            android:rotation="-90" />
        <TextView
            android:id="@+id/testCount"
            android:layout_width="wrap_content"            android:layout_height="wrap_content"
            android:text="0"
            android:textSize="36sp"
            android:textColor="#880000"
            android:textStyle="bold" />
        <TextView
            android:id="@+id/testStatus"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
    android:text="No Test Today"
            android:textSize="18sp"
  android:layout_marginTop="8dp" />
    </LinearLayout>
    <!-- Add Test Button -->
 <Button
    android:id="@+id/addTestButton"
        android:layout_width="200dp"    android:layout_height="wrap_content"
        android:text="Add Test"
        android:layout_below="@id/progressLayout"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp"
        android:backgroundTint="#D32F2F"
    android:textColor="#FFFFFF" />


</RelativeLayout>
 
MAIN ACTIVITY 5
JAVA CODE
package com.example.medcotrack;
import android.app.DatePickerDialog;
import android.content.Context;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.TextView;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Locale;
import java.util.Map;
public class MainActivity5 extends AppCompatActivity {
    private Map<CheckBox, TextView> testDateMap = new LinkedHashMap<>();
    private SharedPreferences sharedPreferences;
   private final String PREF_NAME = "TestPrefs";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);                setContentView(R.layout.activity_main5);
        sharedPreferences = getSharedPreferences(PREF_NAME, MODE_PRIVATE);
        setupTests();
        loadSavedSelections();
        Button nextButton = findViewById(R.id.nextButton);
        nextButton.setOnClickListener(v -> {
            saveSelections();
            startActivity(new Intent(MainActivity5.this, MainActivity6.class));        });
    }
    private void setupTests() {
        addTest(R.id.test1, R.id.date1);
        addTest(R.id.test2, R.id.date2);
        addTest(R.id.test3, R.id.date3);
        addTest(R.id.test4, R.id.date4);
        addTest(R.id.test5, R.id.date5);
        addTest(R.id.test6, R.id.date6);
        addTest(R.id.test7, R.id.date7);
        addTest(R.id.test8, R.id.date8);
        addTest(R.id.test9, R.id.date9);
    addTest(R.id.test10, R.id.date10);
    }
    private void addTest(int checkBoxId, int dateViewId) {
        CheckBox checkBox = findViewById(checkBoxId);
        TextView dateView = findViewById(dateViewId);
        testDateMap.put(checkBox, dateView);
        checkBox.setOnCheckedChangeListener((buttonView, isChecked) -> {
            if (isChecked) {
                showDatePicker(dateView);
            } else {
      dateView.setText("Date: None");
            }
        });
    }
    private void showDatePicker(TextView dateView) {
        Calendar calendar = Calendar.getInstance();
        DatePickerDialog dialog = new DatePickerDialog(this,
     (view, year, month, dayOfMonth) -> {
                    String dateStr = dayOfMonth + "/" + (month + 1) + "/" + year;
                    dateView.setText("Date: " + dateStr);
                },
                calendar.get(Calendar.YEAR),
                calendar.get(Calendar.MONTH),
                calendar.get(Calendar.DAY_OF_MONTH));
        dialog.show();
    }
    private void saveSelections() {
        SharedPreferences.Editor editor = sharedPreferences.edit();
        editor.clear();
        int count = 0;
        for (Map.Entry<CheckBox, TextView> entry : testDateMap.entrySet()) {
            CheckBox checkBox = entry.getKey();
            TextView dateView = entry.getValue();
            if (checkBox.isChecked() && !dateView.getText().toString().equals("Date: None")) {
                editor.putString(checkBox.getText().toString(), dateView.getText().toString());
                count++;
            }
        }
        editor.putInt("testCount", count);  // For progress bar
        editor.apply();
    }
 private void loadSavedSelections() {
        for (Map.Entry<CheckBox, TextView> entry :
testDateMap.entrySet()) {
            CheckBox checkBox = entry.getKey();
            TextView dateView = entry.getValue();
            String testName = checkBox.getText().toString();
            if (sharedPreferences.contains(testName)) {
checkBox.setChecked(true);
                dateView.setText(sharedPreferences.getString(testName, "Date: None"));
            }
        }
    }
}
XML CODE
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        <!-- Logo centered -->
        <ImageView
            android:id="@+id/appLogo"
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:layout_gravity="center"
  android:layout_marginTop="16dp"
            android:src="@drawable/medcotackk"
            android:contentDescription="App Logo" />
        <!-- App name centered -->
        <TextView
            android:id="@+id/appName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:text="MedcoTrack"
            android:textSize="24sp"
            android:textStyle="bold"
            android:textColor="#000000"
            android:layout_marginBottom="24dp" />
        <!-- List of tests and dates -->
        <LinearLayout
            android:orientation="vertical"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <!-- Repeating structure for each test -->
     <CheckBox android:id="@+id/test1" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Blood Test" />
            <TextView android:id="@+id/date1" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />
            <CheckBox android:id="@+id/test2" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Urine Test" />
            <TextView android:id="@+id/date2" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />
   <CheckBox android:id="@+id/test3" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="ECG" />    <TextView android:id="@+id/date3" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />            <CheckBox android:id="@+id/test4" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="X-Ray" />
            <TextView android:id="@+id/date4" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />
    <CheckBox android:id="@+id/test5" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="MRI Scan" />
            <TextView android:id="@+id/date5"
android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />
            <CheckBox android:id="@+id/test6" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="CT Scan" />
            <TextView android:id="@+id/date6" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />
            <CheckBox android:id="@+id/test7" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Eye Checkup" />
  <TextView android:id="@+id/date7" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />
      <CheckBox android:id="@+id/test8" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Dental Checkup" /         <TextView android:id="@+id/date8" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />
            <CheckBox android:id="@+id/test9" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Thyroid Test" />
            <TextView android:id="@+id/date9" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />
            <CheckBox android:id="@+id/test10" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Liver Function Test" />
  <TextView android:id="@+id/date10" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Date: None" />
        </LinearLayout>
    <!-- Next button at bottom -->
        <Button
            android:id="@+id/nextButton"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Next"
            android:layout_marginTop="24dp"
            android:backgroundTint="#4CAF50"
            android:textColor="#FFFFFF" />
    </LinearLayout>
</ScrollView>
      
   
MAIN ACTIVITY 6
JAVA CODE
package com.example.medcotrack;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.widget.ImageView;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
public class MainActivity6 extends AppCompatActivity {
 private TextView upcomingTestsTextView;
    private SharedPreferences sharedPreferences;
    private final String PREF_NAME = "MedcoPrefs";
    private final String SELECTED_TESTS_KEY = "SelectedTests";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main6);
    // Display app name and logo
        TextView appName = findViewById(R.id.appName);
   ImageView appLogo = findViewById(R.id.appLogo);
        appName.setText("MedcoTrack");  // Set your app name
        upcomingTestsTextView = findViewById(R.id.upcomingTestsTextView);
        sharedPreferences = getSharedPreferences(PREF_NAME, MODE_PRIVATE);
        loadUpcomingTests();
    }
    private void loadUpcomingTests() {
        String savedTests = sharedPreferences.getString(SELECTED_TESTS_KEY, "");
    if (!savedTests.isEmpty()) {
            upcomingTestsTextView.setText("Upcoming Tests\n\n" savedTests);
   } else {
            upcomingTestsTextView.setText("Upcoming Tests\n\nNo tests scheduled.");
        }
    }
}
XML CODE
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main6Layout"    android:layout_width="match_parent"    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="20dp"
  android:gravity="center_horizontal"    android:background="#FFFFFF">
   <!-- App Logo and Name -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:orientation="horizontal"
        android:layout_marginBottom="16dp">
        <ImageView
            android:id="@+id/appLogo"
            android:layout_width="48dp"
            android:layout_height="48dp"
            android:src="@drawable/medcotackk"
            android:contentDescription="App Logo"
            android:layout_marginEnd="8dp" />
        <TextView
            android:id="@+id/appName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="MedcoTrack"
            android:textSize="24sp"
            android:textStyle="bold"
            android:textColor="#000000"/>
    </LinearLayout>
    <!-- Upcoming Tests Display -->
    <TextView
        android:id="@+id/upcomingTestsTextView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Upcoming Tests"
        android:textSize="18sp"
        android:textStyle="bold"
        android:gravity="start"
        android:textColor="#333333" />
</LinearLayout>
ANDROID MANIFEST. XML CODE:
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.medcotrack">
    <!-- Permission to post notifications (for Android 13 and above) -->
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
    <application
android:allowBackup="true"        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MedcoTrack">
        <activity
    android:name=".MainActivity5"
   android:exported="false" /> <!-- BroadcastReceiver for Test Notifications -->
        <receiver
            android:name=".NotificationReceiver"
      android:exported="true"
            tools:ignore="ExportedReceiver" /> <!-- Splash Screen -->        <activity
            android:name=".MainActivity"
            android:exported="true">
 <intent-filter>
                <action android:name="android.intent.action.MAIN" />
             <category android:name="android.intent.category.LAUNCHER" />            </intent-filter>
        </activity> <!-- Login Screen -->
        <activity
            android:name=".MainActivity2"
            android:exported="true" /> <!-- Profile Entry -->
        <activity
   android:name=".MainActivity3"
           android:exported="true" /> <!-- Dashboard/Main Screen -->        <activity
            android:name=".MainActivity4"
            android:exported="true" />
        <activity
            android:name=".MainActivity6"/>
    </application>
</manifest>
