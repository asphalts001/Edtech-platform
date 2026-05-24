# Legacy Student

Legacy Student is a modern Android application designed for students to track their academic progress, access resources, and stay synced with their classroom activities.

##  Features

- **Dashboard**: Quick overview of progress, recent test results, and new resources.
- **Tests & Results**: View assigned tests and detailed performance results with a "motivation" system.
- **Leaderboards**: See where you stand among your peers for specific tests.
- **Resources**: Access study materials (PDFs, Videos, Assignments) with offline support.
- **Offline-First**: Powered by Room database and WorkManager for seamless usage without internet.
- **Secure Sync**: Integration with `CoreSync` for reliable data synchronization.
- **Secure Auth**: Encrypted token management for student sessions.

##  Tech Stack

- **UI**: [Jetpack Compose](https://developer.android.com/jetpack/compose) - Modern declarative UI.
- **Dependency Injection**: [Hilt](https://developer.android.com/training/dependency-injection/hilt-android) - Standardized DI for Android.
- **Local Database**: [Room](https://developer.android.com/training/data-storage/room) - Robust offline caching.
- **Networking**: [Retrofit](https://square.github.io/retrofit/) & [OkHttp](https://square.github.io/okhttp/) - Type-safe REST client.
- **Background Tasks**: [WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager) - Guaranteed sync execution.
- **Image Loading**: [Coil](https://coil-kt.github.io/coil/) - Kotlin-first image loading library.
- **Security**: [EncryptedSharedPreferences](https://developer.android.com/topic/security/data) - Secure session management.

## 📁 Project Structure

```text
app/src/main/java/com/legacy/student/
├── data/           # Data layer (Local Room DB, Remote API, Repositories)
├── di/             # Hilt modules
├── domain/         # Domain models and Business logic
├── navigation/     # Jetpack Compose Navigation Graph
├── ui/             # UI layer (Screens, Components, Theme, State)
├── utils/          # Utility classes (Mappers, TokenManager, etc.)
└── worker/         # WorkManager background sync workers
```

