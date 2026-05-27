# Safety Planning Android App

## Purpose & Video Demonstration

This Android application is designed to help users in abusive relationships build and manage a personalized safety plan. Core features include a dynamic questionnaire, secure authentication (including PIN setup), emergency exit, structured storage for emergency information (contacts, documents, safe locations, medications), and quick access to local support resources. All sensitive data is managed via Firebase Authentication, Realtime Database, and Storage.

https://github.com/user-attachments/assets/45974c8e-6764-4d32-9863-8748c1806ec4

## Essential Features

- **PIN protection**: returning users can unlock the app with a locally stored PIN encrypted through Android Keystore.
- **Dynamic questionnaire**: questions are loaded from `app/src/main/assets/questions.json` and are split into warm-up, branch-specific, and follow-up sections.
- **Personalized safety plan**: submitted questionnaire answers are saved to Firebase and transformed into safety tips.
- **Emergency information storage**: users can manage emergency contacts, safe locations, medications, and important documents.
- **Document upload and download**: files can be uploaded to Firebase Storage, saved with metadata, edited, deleted, and downloaded.
- **Local support directory**: support services are loaded from `app/src/main/assets/services_directory.json` and filtered by the user's selected city.
- **Emergency exit**: a floating action button opens Google in the browser, clears the app task, and removes the app from recents.


## Technical Architecture

| Component | Technology / Implementation |
| --- | --- |
| **User Interface (Visuals)** | XML Layouts (`LinearLayout`, `RecyclerView`) |
| **Client-Side Logic**| Java (Activities, Fragments, Adapters), Android SDK |
| **Backend & Database** | Firebase Authentication, Firebase Realtime Database |
| **Cloud Storage** | Firebase Storage |
| **Security** | AndroidX Security (Encrypted `SharedPreferences`, Keystore) |
| **Design Pattern** | Model-View-Presenter (MVP) specifically applied to the login module |
| **Testing** | JUnit, Mockito, AndroidX Test, Espresso |



## Project Structure

```text
ProjectB07/
|-- app/
|   |-- build.gradle.kts
|   |-- google-services.json
|   |-- proguard-rules.pro
|   `-- src/
|       |-- main/
|       |   |-- AndroidManifest.xml
|       |   |-- assets/
|       |   |   |-- questions.json
|       |   |   `-- services_directory.json
|       |   |-- java/com/group15/b07project/
|       |   |   |-- *Activity.java
|       |   |   |-- *Fragment.java
|       |   |   |-- *Adapter.java
|       |   |   `-- model/helper classes
|       |   `-- res/
|       |       |-- drawable/
|       |       |-- font/
|       |       |-- layout/
|       |       |-- values/
|       |       `-- xml/
|       |-- test/
|       `-- androidTest/
|-- docs/
|   `-- PULL_REQUEST_TEMPLATE.md
|-- gradle/
|   `-- libs.versions.toml
|-- build.gradle.kts
|-- settings.gradle.kts
`-- README.md
```

## Key App Components

### Activities

- `LaunchActivity`: app launcher; decides whether to show login or PIN/auth choice.
- `LoginActivity`, `SignupActivity`, `ResetActivity`: Firebase email/password authentication flows.
- `AuthChoiceActivity`, `PinLoginActivity`, `PinSetupActivity`: PIN setup and unlock flow.
- `DisclaimerActivity`: stores per-user disclaimer acceptance.
- `MainActivity`: hosts the main fragments and emergency exit button.

### Fragments

- `HomeFragment`: main navigation screen.
- `QuestionnaireFragment`: renders questionnaire pages and saves answers.
- `PlanGenerationFragment`: generates the personalized safety plan from saved answers.
- `StorageOfEmergencyInfoFragment`: menu for emergency information modules.
- `EmergencyContactsFragment`: add, edit, delete, and list emergency contacts.
- `SafeLocationsFragment`: add, edit, delete, and list safe locations.
- `MedicationsFragment`: add, edit, delete, and list medications.
- `DocumentsToPackFragment`: upload, view, edit, download, and delete document files.
- `SupportConnectionFragment`: displays city-based support resources.

### Helpers and Models

- `PinManager`: encrypts, stores, retrieves, and verifies user PINs.
- `FirebaseFileHelper`: uploads files to Firebase Storage and stores document metadata.
- `ParseJson`: loads JSON assets into Java model objects.
- `LoginContract`, `LoginModel`, `LoginPresenter`: MVP-style login logic.
- `Question`, `QuestionsBundle`, `ServiceDirectory`, `ServiceEntry`, `EmergencyContact`, `SafeLocation`, `Medication`, `DocsDataStructure`, and `DocMetadataStructure`: data models used across the app.

## Firebase Data Layout

The app stores user-scoped data under the current Firebase UID:

```text
users/{uid}/newUser
users/{uid}/questionnaire
users/{uid}/EmergencyInfo/EmergencyContacts
users/{uid}/EmergencyInfo/SafeLocations
users/{uid}/EmergencyInfo/Medications
users/{uid}/Documents/{fileId}
```

Uploaded document files are stored in Firebase Storage:

```text
Documents/{fileId}.{extension}
```

Document metadata stored in Realtime Database includes title, description, upload date, download URL, and storage path.


## Setup

1. Open the project root in Android Studio.
2. Sync Gradle.
3. Ensure `app/google-services.json` is present and configured for package `com.group15.b07project`.
4. In Firebase, enable Email/Password authentication.
5. Configure Firebase Realtime Database and Firebase Storage for the project.
6. Run the `app` configuration on an emulator or Android device running API 24 or newer.

Android Studio will generate `local.properties` for the local SDK path. Do not commit local machine paths or generated build output.

## Legal & Safety Disclaimer

**This app is not a substitute for emergency services.** Safety plans are personal and not guaranteed to prevent harm. If you are in immediate danger, please contact 911 or your local emergency services directly.
