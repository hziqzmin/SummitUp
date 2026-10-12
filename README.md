# SummitUp

> **Offline-first Android hiking journal and social discovery platform built with Kotlin, Jetpack Compose, Room DB, Cloudinary, Firebase, and Naver Map SDK.**

[![Kotlin](https://img.shields.io/badge/Kotlin-100%25-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)](https://kotlinlang.org/)
[![Jetpack Compose](https://img.shields.io/badge/UI-Jetpack%20Compose%20%26%20M3-4285F4?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com/jetpack/compose)
[![Platform](https://img.shields.io/badge/Android-15%20(API%2035)%20%7C%2016%20(API%2036)-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com)
[![Firebase](https://img.shields.io/badge/Cloud-Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/)

SummitUp is a native Android hiking journal and route-sharing platform designed for outdoor enthusiasts. Mountain trails frequently suffer from unstable or nonexistent cellular coverage and traditional cloud-dependent applications often fail in the field. SummitUp solves this through a robust offline-first architecture, allowing hikers to document detailed trail logs, pin geographic coordinates, format descriptions, and attach photos without an internet connection. Once network connectivity is restored, the app automatically executes background cloud synchronization, enabling users to transform their field notes into interactive, shareable trail guides for friends and the outdoor community.

---

## Deliverables & Downloads

Access the complete project assets via the Google Drive folder below:

👉 **[Access SummitUp Project Drive (APK, Demo Video, Technical Report)](https://drive.google.com/drive/folders/1qrVN1SDkxFfJ078Ss6gHBCsxGwyd2ZGD?usp=sharing)**

* **Compiled Application Build:** `SummitUp.apk`
* **Full Demonstration Video:** `SummitUp_Demo_Video.mov`
* **Complete Technical Report (15 Pages):** `SummitUp_ Technical Architecture & Final Project Report.pdf`

---

## Core Features

* **Offline-First Journaling:** Record trail notes, difficulty ratings, rich descriptions, and trail photos locally using **Room Database** (`isSynced = false`). Operates 100% offline.
* **Automated Two-Way Synchronization:** Background coroutines upload media to **Cloudinary CDN**, commit structured documents to **Firebase Firestore**, and update local Room records to `isSynced = true` when network connectivity returns.
* **Interactive Trail Mapping:** Integrated **Naver Map Compose SDK** and Android Geocoder API to render trail pins, waypoints, and geographic coordinates.
* **Reactive Social Discovery Feed:** Powered by Kotlin Coroutines `callbackFlow` to adapt Firebase Firestore snapshot listeners into cold asynchronous reactive streams, rendering live updates directly in Compose `LazyColumn`.
* **Universal Deep Linking & Web Fallback:** Shared hike links navigate to `https://summitup-c4b4a.web.app/hike/{id}`. Handled seamlessly by Android **Verified App Links** (`autoVerify="true"`), routing app users directly to the specific hike screen, while non-app users receive a preview page hosted on **Firebase Hosting**.
* **Modern Identity Management:** Utilizes **Android Credentials Manager** (`androidx.credentials`) supporting modern Google Sign-In and Passkeys alongside Firebase Auth.

---

## Architecture & Engineering Design

SummitUp adheres strictly to Android Modern Architecture recommendations, utilizing **MVVM** and **Unidirectional Data Flow (UDF)**:

```
[ 1. UI Layer (Jetpack Compose Screens) ]
      ▲                               │
      │ State flows UP                │ User actions flow DOWN
  (UiState)                           │ (UiActions / Lambdas)
      │                               ▼
[ 2. ViewModel Layer (Business Logic & State Management) ]
      ▲                               │
      │ Data flows UP                 │ Queries / Requests flow DOWN
      │                               ▼
[ 3. Data Layer (Repository - Single Source of Truth) ]
      ├── Local Storage: Room DB (Offline cache)
      └── Remote Cloud: Firebase Firestore & Cloudinary (Online sync)
```

### Architectural Highlights:
1. **Unidirectional Data Flow (UDF):** ViewModels expose a single immutable `UiState` via `StateFlow` to the UI, while user interactions are dispatched back to ViewModels as a clean set of `UiAction` lambdas.
2. **DTO vs. Entity Separation:** Remote data contracts (`HikeDto`) are strictly decoupled from local database tables (`HikeEntity`), mapped via isolated transformation functions.
3. **Reactive Concurrency:** Utilizes Kotlin `callbackFlow` to adapt Firebase snapshot listeners into asynchronous Flow streams for clean lifecycle-aware UI collection.
4. **Type-Safe Compose Navigation:** Navigation routes are strictly defined using Kotlin `@Serializable` data classes and objects instead of legacy string paths.
5. **Decoupled Media Pipeline:** Media files are uploaded directly to Cloudinary CDN to prevent Firestore document bloat and bandwidth bottlenecks.

---

## Technology Stack & Environment

| Component | Technology |
| :--- | :--- |
| **Language & Tooling** | Kotlin (100%), Kotlin DSL (`.kts`), Version Catalogs (`libs.versions.toml`) |
| **Platform Target** | Minimum SDK: API 35 (Android 15) \| Target SDK: API 36 |
| **UI Framework** | Jetpack Compose, Material 3 (M3), RichEditor Compose |
| **Architecture** | MVVM, Unidirectional Data Flow (UDF), Repository Pattern |
| **Concurrency** | Kotlin Coroutines, StateFlow, SharedFlow, `callbackFlow` |
| **Local Persistence** | Android Room Database, SQLite |
| **Cloud & Backend** | Firebase Firestore (NoSQL), Firebase Authentication, Firebase Hosting |
| **Media & CDN** | Cloudinary Android SDK, Coil 3 (Asynchronous Image Loading) |
| **Maps & Location** | Naver Map Compose SDK, Android Location Services, Geocoder API |
| **Identity & Security** | Android Credentials Manager (Passkeys/Google Sign-In), Secrets Gradle Plugin |

---

## Academic Attribution

* **Institution:** Konkuk University, Seoul, South Korea
* **Department:** Department of Computer Science & Engineering
* **Course:** Mobile Programming
* *Academic note: The original course repository was managed within an institutional classroom organization and archived upon semester evaluation.*
