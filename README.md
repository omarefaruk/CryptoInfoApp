# Crypto Info App

An Android app for browsing cryptocurrency information, built with Kotlin and Jetpack Compose using Clean Architecture, MVVM and tests across every layer.

## Features

- Browse cryptocurrencies by market rank, with active/inactive status
- Coin detail screen with description, tags and team members
- Loading and error states for network requests
- Material 3 UI built entirely in Jetpack Compose

Data comes from the public [CoinPaprika API](https://api.coinpaprika.com/).

## Architecture

The app follows Clean Architecture with three layers:

```
presentation/   Compose screens, ViewModels (MVVM), navigation
domain/         Use cases, domain models, repository interfaces
data/           Retrofit API, DTOs, repository implementation
```

- **MVVM:** ViewModels expose UI state to Compose screens
- **Use cases:** `GetCoinsUseCase` and `GetCoinUseCase` hold the business logic and emit `Resource` states (Loading / Success / Error) via Kotlin Flow
- **Repository pattern:** the domain layer depends on an interface, so the data source can be swapped for fakes in tests
- **Dependency injection:** Hilt, with a separate test module for instrumented tests

## Tech stack

Kotlin · Jetpack Compose · Material 3 · Coroutines & Flow · Hilt (KSP) · Retrofit · OkHttp · Gson · Navigation Compose

## Testing

| Layer | What's tested | Tools |
|---|---|---|
| Domain | Use cases emit the correct Loading → Success / Error sequence | JUnit, MockK, `runTest` |
| Presentation | ViewModels update state correctly from use-case results | JUnit, MockK, fake repository, `MainCoroutineRule` |
| UI | Coin list and detail screens render the expected content | Compose UI testing, Hilt testing |
| End-to-end | Full journey from list to detail screen against a mock server | Compose UI testing, MockWebServer |

## Running the project

Requirements: Android Studio (recent version), JDK 17, Android SDK API 24+

```bash
git clone https://github.com/omarefaruk/CryptoInfoApp.git
```

Open the project in Android Studio and run the `app` configuration.

```bash
./gradlew test                  # unit tests
./gradlew connectedAndroidTest  # UI and end-to-end tests (needs an emulator or device)
```

## What I'd add next

- Local caching with Room so the coin list is available offline
- Expose ViewModel state as `StateFlow`
- A CI workflow (GitHub Actions) to run the tests on every push
