# Pokédex Android App

A native Android application written in Kotlin for browsing Pokémon, searching by name, and viewing individual information cards. Pokémon data is stored locally in SQLite, with images and type icons bundled in the application.

## Features

- Scrollable Pokémon list built with `RecyclerView`, showing each Pokémon's number, name, image, and type icons.
- Live, case-insensitive name search with a button to clear the filter.
- Detail screen displaying a Pokémon's image, number, name, description, and one or two types.
- Local SQLite database initialized with a bundled dataset on first use.
- Navigation through an `Intent` carrying only the Pokémon ID; the detail activity retrieves the corresponding record from SQLite.
- Local data and image loading without a remote API or an internet connection at runtime.

## Technology

| Component | Implementation |
| --- | --- |
| Language | Kotlin |
| UI | Android XML layouts, AndroidX AppCompat, RecyclerView, ConstraintLayout |
| Storage | SQLite through `SQLiteOpenHelper` |
| Navigation | Activities and Android intents |
| Images | PNG resources in `app/src/main/assets/` |
| Build | Gradle Kotlin DSL, Gradle wrapper 9.1.0, Android Gradle Plugin 9.0.1 |
| Android support | Minimum API 24 (Android 7.0); compile and target API 36 |

The SDK settings above describe this repository. They differ from the minimum API 22 used in the assignment handout.

## Getting started

### Requirements

- Android Studio compatible with Android Gradle Plugin 9.0.1.
- JDK 17 or a compatible newer JDK configured as the Gradle JDK. The Java source/target compatibility setting is 11; this is separate from the JDK required to run the build.
- Android SDK Platform 36 and SDK Build Tools 36.0.0.
- An Android emulator or physical device running API 24 or higher.

See the official [Android Gradle Plugin compatibility requirements](https://developer.android.com/build/releases/agp-9-0-0-release-notes#compatibility) for the build environment.

### Run in Android Studio

1. Clone the repository:

   ```sh
   git clone https://github.com/OUNZAR-Aymane/Pokedex.git
   cd Pokedex
   ```

2. Open the repository root in Android Studio.
3. Install any required SDK components and allow Gradle to synchronize.
4. Select an emulator or connect a device with USB debugging enabled.
5. Run the `app` configuration. Use portrait orientation for the detail screen; see the current limitations below.

No API key or external database setup is required. Internet access is needed initially to download build dependencies and SDK components.

## Project structure

```text
app/src/main/
├── AndroidManifest.xml
├── java/fr/imt/pokedex/
│   ├── PokedexListActivity.kt       # List display and live name filtering
│   ├── PokemonCardActivity.kt       # Detail loading and display
│   ├── adapter/
│   │   └── PokemonListAdapter.kt    # List binding and navigation
│   ├── data/
│   │   ├── DBHelper.kt              # SQLite schema, seed data, and queries
│   │   └── Datasource.kt            # Earlier nine-Pokémon sample dataset
│   └── model/
│       ├── Pokemon.kt               # Pokémon data class
│       └── Type.kt                  # Pokémon type enum
├── assets/                         # Pokémon images and type icons
└── res/
    ├── layout/                     # List, item, and detail layouts
    └── layout-land/                # Landscape detail layout placeholder
```

## Data flow

1. `PokedexListActivity` reads the Pokémon list using `DBHelper.getAllPokemons()`.
2. Search text filters that list in memory and refreshes the adapter.
3. Selecting a list item opens `PokemonCardActivity` with a `pokemonId` extra.
4. The detail activity calls `DBHelper.getPokemonById()` and loads the matching images from assets.

The `Pokemon` model stores an ID, name, description, primary type, and optional secondary type. Pokémon images use zero-padded IDs such as `001.png`; type icons use enum names such as `GRASS.png`.

The database is named `pokemons.db`. Its current version is 3. Upgrades and downgrades drop and recreate the Pokémon table from the bundled seed data.
