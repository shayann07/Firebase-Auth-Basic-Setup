# Firebase Authentication Android Starter

[![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com)
[![Language](https://img.shields.io/badge/Kotlin-2.0+-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![Authentication](https://img.shields.io/badge/Firebase-Authentication-FFCA28?style=for-the-badge&logo=firebase)](https://firebase.google.com/docs/auth)
[![Database](https://img.shields.io/badge/Firebase-Cloud%20Firestore-FFCA28?style=for-the-badge&logo=firebase)](https://firebase.google.com/docs/firestore)
[![UI](https://img.shields.io/badge/UI-ViewBinding-FF4081?style=for-the-badge&logo=android)](https://developer.android.com/topic/libraries/view-binding)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

> A clean, production-ready Android starter boilerplate implementing Google Firebase Authentication (Email/Password registration, sign-in, auto-login session listeners, and sign-out) with ViewBinding and Cloud Firestore readiness.

---

## 📖 Overview

The **Firebase-Auth-Basic-Setup** project provides an essential foundation for Android applications requiring user identity management. Utilizing official Firebase SDKs, it implements standard authentication flows, session state persistence, and lifecycle listener monitoring in Kotlin with minimal boilerplate.

### 🎯 Key Learning Objectives
- **Firebase Authentication Integration**: Initializing and managing `FirebaseAuth` instances across multiple Activities.
- **User Registration & Validation**: Creating new user accounts via `createUserWithEmailAndPassword` with client-side field validation and password confirmation matching.
- **User Sign-In & Verification**: Authenticating credentials with `signInWithEmailAndPassword` and handling asynchronous success/failure callbacks.
- **Session State Monitoring (`onStart`)**: Inspecting `auth.currentUser` across Activity lifecycles to automatically route logged-in users to the main dashboard or unauthenticated users to the login screen.
- **Clean Architecture with ViewBinding**: Utilizing type-safe view binding and modern Android 15 / SDK 36 standards.

---

## 🏗️ Authentication Lifecycle & Routing Flow

```mermaid
graph TD
    classDef start fill:#1A365D,stroke:#63B3ED,stroke-width:2px,color:#fff;
    classDef auth fill:#7B341E,stroke:#ED8936,stroke-width:2px,color:#fff;
    classDef success fill:#234E52,stroke:#38B2AC,stroke-width:2px,color:#fff;
    classDef ui fill:#2D3748,stroke:#4FD1C5,stroke-width:2px,color:#fff;

    AppLaunch["App Launch / Activity Start"]:::start --> CheckAuth{"auth.currentUser != null?"}

    CheckAuth -->|Yes (Session Active)| MainActivity["MainActivity<br/>(Welcome Screen)"]:::success
    CheckAuth -->|No (Unauthenticated)| LoginActivity["LoginActivity<br/>(Sign In Screen)"]:::ui

    LoginActivity -->|User enters email & pass| SignInAction["auth.signInWithEmailAndPassword()"]:::auth
    LoginActivity -->|Navigate| RegisterActivity["RegisterActivity<br/>(Sign Up Screen)"]:::ui

    RegisterActivity -->|Validate & Submit| CreateUserAction["auth.createUserWithEmailAndPassword()"]:::auth

    SignInAction -->|Success| MainActivity
    CreateUserAction -->|Success| MainActivity

    MainActivity -->|User taps 'Log Out'| SignOutAction["auth.signOut()"]:::auth
    SignOutAction --> LoginActivity
```

---

## ✨ Core Concepts & Implementation

### 1. Persistent Session Checking (`onStart`)
Verifying the user authentication token on every activity startup prevents unauthenticated access:
```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var auth: FirebaseAuth

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        auth = FirebaseAuth.getInstance()
        val email = auth.currentUser?.email ?: "Unknown"
        binding.tvWelcome.text = "Welcome, $email"

        binding.btnLogout.setOnClickListener {
            auth.signOut()
            startActivity(Intent(this, LoginActivity::class.java))
            finish()
        }
    }

    override fun onStart() {
        super.onStart()
        if (auth.currentUser == null) {
            startActivity(Intent(this, LoginActivity::class.java))
            finish()
        }
    }
}
```

### 2. User Sign-Up with Confirmation Validation
Validating password consistency before initiating remote Firebase authentication tasks:
```kotlin
private fun register() {
    val email = binding.etEmail.text.toString()
    val password = binding.etPassword.text.toString()
    val confirmPassword = binding.etConfirm.text.toString()

    if (email.isEmpty() || password.isEmpty() || confirmPassword.isEmpty()) {
        Toast.makeText(this, "Please enter all fields", Toast.LENGTH_SHORT).show()
        return
    }
    if (password != confirmPassword) {
        Toast.makeText(this, "Passwords do not match", Toast.LENGTH_SHORT).show()
        return
    }

    auth.createUserWithEmailAndPassword(email, password).addOnCompleteListener { task ->
        if (task.isSuccessful) {
            Toast.makeText(this, "Registration successful", Toast.LENGTH_SHORT).show()
            startActivity(Intent(this, MainActivity::class.java))
            finish()
        } else {
            Toast.makeText(this, "Registration failed", Toast.LENGTH_SHORT).show()
        }
    }
}
```

---

## 📱 Key Components & Project Structure

```
Firebase-Auth-Basic-Setup/
├── app/
│   ├── src/main/java/com/shayan/firebaseauthsetup/
│   │   ├── LoginActivity.kt               # Login screen & auto-redirect logic
│   │   ├── RegisterActivity.kt            # Account creation & password validation
│   │   └── MainActivity.kt                # Protected dashboard with sign-out handler
│   ├── src/main/res/
│   │   ├── layout/                        # XML layouts for Login, Register, Main
│   │   └── values/                        # Color palettes, string resources & themes
│   ├── build.gradle.kts                   # Target SDK 36, Firebase Auth & Firestore dependencies
│   └── google-services.json               # (Template) Firebase configuration file
└── gradle/
    └── libs.versions.toml                 # Centralized dependency catalog
```

---

## 🛠️ Technology Stack Matrix

| Layer | Technology | Details |
|---|---|---|
| **Platform** | Android | API 24+ (Compile & Target SDK 36) |
| **Language** | Kotlin | 2.0+ |
| **Authentication** | Firebase Auth | `com.google.firebase:firebase-auth` |
| **Database Readiness** | Cloud Firestore | `com.google.firebase:firebase-firestore` |
| **Google Services** | Google Services Plugin | `com.google.gms.google-services` |
| **UI Framework** | Android XML + ViewBinding | Null-safe view referencing |
| **Design** | Material Design Components | `com.google.android.material:material` |

---

## 🚀 Getting Started

### Prerequisites
- **Android Studio Ladybug (2024.2.1+)** or newer.
- **JDK 11 or JDK 17**.
- **A Google Firebase Project**:
  1. Go to the [Firebase Console](https://console.firebase.google.com).
  2. Create a new project and add an Android app with package name `com.shayan.firebaseauthsetup`.
  3. Enable **Email/Password** sign-in method in Firebase Console under Authentication > Sign-in method.
  4. Download `google-services.json` and place it in the `app/` root directory.

### Build & Run
1. **Clone the repository**:
   ```bash
   git clone https://github.com/shayann07/Firebase-Auth-Basic-Setup.git
   cd Firebase-Auth-Basic-Setup
   ```
2. **Add `google-services.json`**: Place your Firebase credentials file in the `app/` directory.
3. **Build and install**:
   ```bash
   ./gradlew assembleDebug
   ```
4. **Deploy**: Run on an Android emulator or physical device.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE) — Copyright (c) 2026 [shayann07](https://github.com/shayann07).
