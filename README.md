# Vigyáz(z) 6! – Score Keeper for Android

**English** | [Magyar](README.hu.md)

A native Android app that keeps score for the **Vigyázz 6!** card game (known internationally as *6 nimmt!* / *Take 6!*). Enter the bull heads each player collects round by round; the app deducts the points, ranks the players, detects the end of the game, highlights the winner and keeps the history.

It works completely offline: the app has **no internet permission**, collects no data and contains no ads.

> Google Play release in preparation.

<p align="center">
  <img src="screenshots/welcome.jpg" alt="Welcome screen" width="180">
  &nbsp;
  <img src="screenshots/game.jpg" alt="Live ranking during the game" width="180">
  &nbsp;
  <img src="screenshots/round-entry.jpg" alt="Entering the points of a round" width="180">
  &nbsp;
  <img src="screenshots/game-over.jpg" alt="Game over with the winner" width="180">
  &nbsp;
  <img src="screenshots/history.jpg" alt="History and win statistics" width="180">
</p>

## Features

- **Player list** – add and remove players (2–10); the list is kept between games, and recently used names can be added with one tap.
- **Adjustable starting points** – 66 by default; every player starts from this value.
- **Round entry on one sheet** – all players in seating order, with − / + buttons or the number keyboard, and a live preview of the new score (e.g. `28 → 18 points`).
- **Game-end warning** – the entry sheet tells you before saving if the round will end the game.
- **Live ranking** – sorted after every round with animation, ties share a rank, and each card shows the points of all previous rounds.
- **Undo** – the last round can be removed if someone mistyped.
- **Game over screen** – the winner (or winners, in case of a tie), final standings, *new game with the same players*, and sharing the result as text to any app.
- **History** – finished games with date, winner and full standings, plus a win counter per player.
- **Built-in rules** of the card game.
- **English, Hungarian and German** interface, with per-app language selection on Android 13+.
- **The screen stays on** while the game screen is open.
- **Adaptive and themed launcher icon.**

## Privacy

All data (players, rounds, finished games) is stored in a local Room database on the device. The app declares no internet permission, so nothing can leave the phone; the only way data leaves is when the user explicitly shares a result through Android's share menu.

Privacy policy: <https://zdomiter.github.io/vigyazz6/privacy.html>

## Tech stack

- Kotlin, Jetpack Compose, Material 3
- Room 2.8 (with KSP and exported schemas)
- Navigation Compose
- ViewModel + StateFlow
- R8 code and resource shrinking for release builds
- minSdk 26 (Android 8.0), targetSdk 36 (Android 16), compileSdk 37

## Architecture

The app stores **the bull heads taken per round**, not the scores. A player's score is always `starting points − sum of bull heads`, so it can never get out of sync, and undoing a round simply deletes its rows.

| Package | Content |
|---------|---------|
| `data` | Room entities (`roster`, `games`, `game_players`, `round_scores`), `GameDao`, `AppDatabase` |
| `game` | `GameLogic` – pure Kotlin scoring, ranking and validation (unit tested); `GameViewModel` |
| `ui/welcome`, `ui/players`, `ui/game`, `ui/history`, `ui/rules` | Screens and their ViewModels |
| `ui/navigation` | Routes, bottom bar, app scaffold |
| `ui/components` | Shared UI: top bar, pattern background, step button, auto-size text, sharing, formatting |
| `ui/theme` | Colours, Rubik typography, tiled pattern brush |

## Building

1. Clone the repository and open it in Android Studio:
   ```bash
   git clone https://github.com/zdomiter/vigyazz6_android.git
   ```
2. Run the `app` configuration on a device or emulator (debug build, no signing setup needed).
3. Run the unit tests:
   ```bash
   ./gradlew test
   ```

### Release build

Release signing reads a `keystore.properties` file from the project root. It is excluded from the repository and must be created locally:

```properties
storeFile=C:/path/to/upload-key.jks
storePassword=...
keyAlias=upload
keyPassword=...
```

Then use *Build → Generate Signed App Bundle or APK*.

## Related

The original web version: [zdomiter/vigyazz6_web](https://github.com/zdomiter/vigyazz6_web)

## Disclaimer

This is an unofficial fan-made scoring app. *6 nimmt!* is a card game designed by Wolfgang Kramer and published by AMIGO; all related trademarks belong to their respective owners.

## Author

© 2026 Domiter Zoltán (Domitersoft) – All rights reserved.
