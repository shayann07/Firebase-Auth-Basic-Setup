# Firebase Auth Basic Setup

This repository demonstrates a basic implementation of Firebase Authentication in an Android app. It provides boilerplate code and simple UI examples to get you up and running quickly with user authentication using Firebase.

## Features

- Email/password registration and sign‑in.
- Persistent user session (auto login).
- Sign out functionality.
- Error handling and input validation.
- Minimal UI built with Android Jetpack components.

## Getting Started

1. Clone this repository:

   ```bash
   git clone https://github.com/shayann07/Firebase-Auth-Basic-Setup.git
   ```

2. Open the project in **Android Studio**.

3. Allow Gradle to download dependencies.

4. Create a new project in the **Firebase console**, enable **Email/Password** sign‑in method, and download the `google-services.json` file into the `app/` directory.

5. Run the app on a device or emulator:

   ```bash
   ./gradlew installDebug
   ```

## Technologies Used

- **Kotlin** – primary language.
- **Firebase Authentication** – email/password sign‑in.
- **Firebase Android SDK** – integration with Google services.
- **Android Jetpack** libraries.

## License

This project is licensed under the MIT License.
