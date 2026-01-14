# Rick and Morty Character Search

[![Kotlin](https://img.shields.io/badge/Kotlin-2.0.21-blue.svg)](https://kotlinlang.org)
[![Android](https://img.shields.io/badge/Android-API%2026+-green.svg)](https://developer.android.com)
[![Compose](https://img.shields.io/badge/Jetpack%20Compose-BOM%202024.09-orange.svg)](https://developer.android.com/jetpack/compose)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An Android application for searching Rick and Morty characters with real-time filtering and smooth animations. Built with modern Android development practices using Jetpack Compose and MVVM architecture.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Building & Running](#building--running)
- [Testing](#testing)
- [Screenshots](#screenshots)
- [Code Highlights](#code-highlights)
- [API](#api)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

## Features

✨ **Core Features:**
- Real-time character search with debounced API calls
- Advanced filtering by status, species, and type
- Grid display of character cards with images
- Detailed character view with comprehensive information
- Animated image transitions between screens
- Share character information via Android share sheet
- Loading states and error handling
- Material Design 3 UI

## Tech Stack

- **Language:** Kotlin
- **UI:** Jetpack Compose with Material3
- **Architecture:** MVVM with Repository pattern
- **Networking:** Retrofit with Kotlinx Serialization
- **Image Loading:** Coil
- **Navigation:** Navigation Compose
- **Async:** Kotlin Coroutines & Flow
- **Testing:** JUnit, MockK, Compose Testing (29 tests total)

## Architecture

```
┌─────────────┐
│  UI Layer   │  SearchScreen, DetailScreen (Jetpack Compose)
└──────┬──────┘
       │
┌──────▼──────┐
│  ViewModel  │  SearchViewModel (StateFlow, Coroutines)
└──────┬──────┘
       │
┌──────▼──────┐
│ Repository  │  CharacterRepository (Data abstraction)
└──────┬──────┘
       │
┌──────▼──────┐
│  API/Data   │  Retrofit, Rick & Morty API
└─────────────┘
```

**Key Design Decisions:**
- **MVVM**: Clear separation of concerns, testable business logic
- **Repository Pattern**: Abstracts data sources, enables caching later
- **StateFlow**: Reactive state management, lifecycle-aware
- **Sealed Interfaces**: Type-safe state handling
- **ViewModelFactory**: Proper dependency injection and lifecycle management

## Building & Running

### Prerequisites
- Android Studio Hedgehog or later
- JDK 11+
- Android SDK (Min API 24)

### Steps
1. Clone the repository
2. Open in Android Studio
3. Sync Gradle files
4. Run on emulator or device

```bash
./gradlew assembleDebug
./gradlew installDebug
```

## Testing

**Unit Tests (5 tests):**
```bash
./gradlew test
```
- Repository API interactions
- Error handling scenarios
- Input validation

**UI Tests (10 tests):**
```bash
./gradlew connectedAndroidTest
```
- Search screen interactions
- Detail screen display
- Navigation flows
- Filter functionality

## Screenshots

<!-- Add screenshots here -->
<!-- 
![Search Screen](screenshots/search.png)
![Detail Screen](screenshots/detail.png)
![Filter Options](screenshots/filters.png)
-->

## Features Checklist

✅ Search bar at top with grid below  
✅ Real-time search updates  
✅ Progress indicator during loading  
✅ Detail view with all required information  
✅ Unit tests  
✅ Kotlin & Jetpack Compose  

**Additional Features:**
✅ UI tests (comprehensive test coverage)  
✅ Share functionality  
✅ Animated image transitions  
✅ Filter options (status, species, type)

## Code Highlights

**Debounced Search:**
```kotlin
fun onSearchQueryChanged(query: String) {
    searchJob?.cancel()
    searchJob = viewModelScope.launch {
        delay(300) // Debounce
        searchCharacters(query)
    }
}
```

**Shared Element Transitions:**
```kotlin
SharedTransitionLayout {
    // Smooth image animations between screens
    .sharedBounds(
        sharedContentState = rememberSharedContentState(key = "image-${character.id}"),
        animatedVisibilityScope = animatedContentScope
    )
}
```

**Type-Safe State Management:**
```kotlin
sealed interface SearchUiState {
    data object Initial : SearchUiState
    data object Loading : SearchUiState
    data class Success(val characters: List<Character>) : SearchUiState
    data object Empty : SearchUiState
    data class Error(val message: String) : SearchUiState
}
```

## API

Uses the free [Rick and Morty API](https://rickandmortyapi.com/):
- **Endpoint:** `GET /character/?name={query}&status={status}&species={species}`
- No API key required
- Supports filtering by name, status, species, and type

## Project Structure

```
app/src/main/java/com/example/cvstakehome/
├── data/
│   ├── api/          # Retrofit interface
│   ├── model/        # Data models
│   └── repository/   # Repository implementation
├── ui/
│   ├── search/       # Search screen & ViewModel
│   ├── detail/       # Detail screen
│   ├── navigation/   # Navigation setup
│   └── theme/        # App theming
└── MainActivity.kt
```

## Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](../../issues).

## License

This project is open source and available under the [MIT License](LICENSE).
