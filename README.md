# Firebase Auth Basic Setup

A small Kotlin Android sample wiring Firebase Authentication email/password sign-in, sign-up, persistent session, and sign-out across three activities.

## Status

Educational sample. Single-module Android app with Firebase Auth and the Firestore Android SDK as a dependency (no Firestore reads or writes are performed in the audited sources).

## Features

- Register a new user with email and password, with confirm-password matching and empty-field validation.
- Log in with email and password using `FirebaseAuth.signInWithEmailAndPassword`.
- Auto-route to `MainActivity` while a Firebase user session exists, and back to `LoginActivity` on sign-out.
- Display the current user's email on the home screen.
- Sign out via `FirebaseAuth.signOut`.
- View binding enabled across all three screens.

## Tech Stack

- **Language:** Kotlin (JVM target 11).
- **UI:** Android Views with AppCompat, Material Components, ConstraintLayout; View Binding (`buildFeatures.viewBinding = true`).
- **Auth/Backend SDKs:** `com.google.firebase:firebase-auth` and `com.google.firebase:firebase-firestore` declared via the project's `libs.versions.toml`.
- **Build:** Android Gradle Plugin and the `com.google.gms.google-services` Gradle plugin (alias `google.gms.google.services`).
- **SDK levels:** `compileSdk 36`, `minSdk 24`, `targetSdk 36`.

## Architecture

- `LoginActivity` is the launcher activity. In `onStart` it forwards to `MainActivity` if `FirebaseAuth.currentUser` is non-null. Login button calls `signInWithEmailAndPassword` and routes to `MainActivity` on success.
- `RegisterActivity` collects email and two password fields, ensures they match, then calls `createUserWithEmailAndPassword` and goes straight to `MainActivity` on success.
- `MainActivity` shows the current user's email, sends sign-outs back to `LoginActivity`, and also redirects to login if the session disappears.
- `FirebaseAuth.getInstance()` is created locally in each activity; there is no shared application class, repository, or view model layer.

## Project Structure

```
app/
├── build.gradle.kts
├── google-services.json          // tracked; required by the google-services plugin
└── src/main/
    ├── AndroidManifest.xml       // INTERNET + ACCESS_NETWORK_STATE; LoginActivity is the launcher
    ├── java/com/shayan/firebaseauthsetup/
    │   ├── LoginActivity.kt
    │   ├── MainActivity.kt
    │   └── RegisterActivity.kt
    └── res/
        ├── layout/
        │   ├── activity_login.xml
        │   ├── activity_main.xml
        │   └── activity_register.xml
        ├── values/
        └── ...
```

## Getting Started

### Prerequisites

- Android Studio with Android Gradle Plugin and Kotlin support compatible with the wrapper version in `gradle/wrapper`.
- JDK 11.
- Android SDK with `compileSdk 36` and `minSdk 24`.
- A Firebase project with Email/Password sign-in enabled.

### Configure Firebase

The repository contains an `app/google-services.json`. To run against your own Firebase project, replace it with the `google-services.json` exported from your project. Sign-in must be enabled under Authentication → Sign-in method → Email/Password in the Firebase console.

### Run

```bash
git clone https://github.com/shayann07/Firebase-Auth-Basic-Setup.git
```

Open in Android Studio, sync Gradle, then run the `app` configuration on a device or emulator. From the command line:

```bash
./gradlew :app:installDebug
```

## Limitations

- Firestore is declared as a dependency but no Firestore reads or writes are performed in the audited sources.
- Login validation only checks "both empty"; one empty field still calls Firebase and shows the generic failure toast.
- All error states surface as a single "Login failed" or "Registration failed" toast without the underlying `task.exception` details.
- A `google-services.json` is committed to the repository; replace it with your own project's file before using.
- Only generated example tests are present, and there is no license file.
