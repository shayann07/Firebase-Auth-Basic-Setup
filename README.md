# Firebase Auth Basic Setup

[![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?logo=android&logoColor=white)]()
[![Language](https://img.shields.io/badge/Language-Kotlin-7F52FF?logo=kotlin&logoColor=white)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> A minimal starter showing how to wire Firebase email-and-password authentication into an Android app â€” sign up, sign in, sign out, and persisted session.

---

## 📖 Overview

A minimal starter showing how to wire Firebase email-and-password authentication into an Android app â€” sign up, sign in, sign out, and persisted session.

---

## ✨ Key Features

- Register a new user with email and password, with confirm-password matching and empty-field validation.
- Log in with email and password using `FirebaseAuth.signInWithEmailAndPassword`.
- Auto-route to `MainActivity` while a Firebase user session exists, and back to `LoginActivity` on sign-out.
- Display the current user's email on the home screen.
- Sign out via `FirebaseAuth.signOut`.
- View binding enabled across all three screens.

---

## 🛠️ Technology Stack

| Component / Layer | Technology |
|---|---|
| **Platform** | Android |
| **Primary Language** | Kotlin |
| **Architecture** | MVVM / Clean Architecture |
| **License** | Open Source (MIT) |

---

## 🚀 Getting Started

### Prerequisites
- Android Studio Ladybug (or newer)
- JDK 17 / 21
- Android SDK 34 / 35

### Build & Run
1. Clone the repository:
   ```bash
   git clone https://github.com/shayann07/Firebase-Auth-Basic-Setup.git
   cd Firebase-Auth-Basic-Setup
   ```
2. Open the project in **Android Studio**.
3. Sync Gradle dependencies and run on an emulator or physical device.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE) — Copyright (c) 2026 [shayann07](https://github.com/shayann07).
