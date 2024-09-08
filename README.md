# Login Demo

## XML code

### Main Activity

```xml
<TextView
    android:id="@+id/textView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginTop="96dp"
    android:text="My App"
    android:textColor="@color/pink"
    android:textSize="24sp"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    tools:ignore="HardcodedText" />

<ImageView
    android:id="@+id/imageView2"
    android:layout_width="256dp"
    android:layout_height="256dp"
    android:layout_marginTop="128dp"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintHorizontal_bias="0.496"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    app:srcCompat="@drawable/avatar" />

<Button
    android:id="@+id/logout_button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Log Out"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@id/imageView2"
    android:layout_marginTop="32dp" />
```

### Login Activity

```xml
<EditText
    android:id="@+id/username"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:layout_marginStart="16dp"
    android:layout_marginTop="256dp"
    android:layout_marginEnd="16dp"
    android:hint="Username"
    android:inputType="text"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintHorizontal_bias="0.0"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    tools:ignore="HardcodedText" />

<EditText
    android:id="@+id/password"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:layout_marginStart="16dp"
    android:layout_marginTop="28dp"
    android:layout_marginEnd="16dp"
    android:hint="Password"
    android:inputType="textPassword"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintHorizontal_bias="0.0"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@id/username"
    tools:ignore="HardcodedText" />

<Button
    android:id="@+id/login_button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginTop="12dp"
    android:text="Log in"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@id/password"
    tools:ignore="HardcodedText" />
```

## Java code

### Main Activity

#### Check is logged in 

Run this first to navigate to Login activity

```java
if (!isLoggedIn()) {
    navigateToLogin();
    return;
}
```

Check login with `SharedPreferences`

```java
private boolean isLoggedIn() {
    SharedPreferences sharedPreferences = getSharedPreferences(AppConstants.PREFERENCES_FILE, Context.MODE_PRIVATE);
    return sharedPreferences.getBoolean(AppConstants.KEY_IS_LOGGED_IN, false);
}
```

Navigate to another activity (Login)

```java
 private void navigateToLogin() {
    Intent intent = new Intent(this, LoginActivity.class);
    startActivity(intent);
    finish();
}
```

#### Log out

Log out method, set the `SharedPreferences` state

```java
private void logout() {
    SharedPreferences sharedPreferences = getSharedPreferences(AppConstants.PREFERENCES_FILE, Context.MODE_PRIVATE);
    SharedPreferences.Editor editor = sharedPreferences.edit();
    editor.putBoolean(AppConstants.KEY_IS_LOGGED_IN, false);
    editor.apply();

    Intent intent = new Intent(this, LoginActivity.class);
    startActivity(intent);
    finish();
}
```

### Login Activity

Take the input and validate credentials

```java
private void handleLogin(View view) {
    String username = usernameEditText.getText().toString().trim();
    String password = passwordEditText.getText().toString().trim();
  
    if (validateCredentials(username, password)) {
        saveLoginState();
        Toast.makeText(LoginActivity.this, "Sign-up successful!", Toast.LENGTH_SHORT).show();
        navigateToHome();
    } else {
        Toast.makeText(LoginActivity.this, "Invalid username or password!", Toast.LENGTH_SHORT).show();
    }
}
```

Simple fixed value credentials validation

```java
private boolean validateCredentials(String username, String password) {
    return !TextUtils.isEmpty(username) && username.equals("user") && password.equals("pass");
}
```

Save the login state to `SharedPreferences`

```java
private void saveLoginState() {
    SharedPreferences sharedPreferences = getSharedPreferences(AppConstants.PREFERENCES_FILE, Context.MODE_PRIVATE);
    SharedPreferences.Editor editor = sharedPreferences.edit();
    editor.putBoolean(AppConstants.KEY_IS_LOGGED_IN, true);
    editor.apply();
}
```
Navigate to another activity (Home)

```java
private void navigateToHome() {
    Intent intent = new Intent(LoginActivity.this, MainActivity.class);
    startActivity(intent);
    finish();
}
```
### App constants

To store universal constants

```java
public class AppConstants {
    public static final String PREFERENCES_FILE = "com.example.layoutdemo.PREFS";
    public static final String KEY_IS_LOGGED_IN = "is_logged_in";
}
```
