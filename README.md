# II.3510_2627_Skills_MLkit

Android demonstration for topic 11: category suggestions from an object photo and barcode reading from a photo. Based on our pair's project: https://github.com/jafarifi/II.3510_2627_Skills_MLkit

## Open and run

Open this folder in Android Studio, wait for Gradle sync and run the `app` module on an Android device or emulator (minimum API 24). Install compile SDK Android 37.0 if requested. The test emulator uses Android API 36; it does not need the same version as the compile SDK.

The `shared` module declares Android and iOS targets. This demonstration provides an Android interface only.

## Test accounts

- Alice: `alice@isep.fr` / `alice123`
- Bob: `bob@isep.fr` / `bob123`
- Administrator: `admin@isep.fr` / `admin123`

Use Alice or Bob to publish a listing. These are fictional test credentials.


## Demonstration

1. Sign in, open **My Listings**, then **Post a listing**.
2. **Suggest category from photo** selects an object photo and suggests a category when a sufficiently confident known label is found.
3. **Scan barcode from photo** selects a separate barcode image. It does not replace the object photo.
4. Check **Barcode / ISBN**, click **Add to description**, complete the listing and press **Save**.

Test images are in `demo/`: `ISBN_demo.png` should produce `9780134685991`, while `No_barcode.png` contains no code. Copy them into the device gallery before the demonstration. Rehearse the complete photo-picker flow on the presentation device.

Configured formats: EAN-13, QR Code and Code 128. Scanning reads a value; it does not look up a book title or price and does not use a continuous camera feed. Listings are kept in memory.

## Code organisation

- `app/src/main/java/com/isep/swapcampus/ListingFormScreen.kt`: Compose form and ViewModel.
- `app/src/main/java/com/isep/swapcampus/ml/ImageLabelerHelper.kt`: ML Kit image labeling calls.
- `app/src/main/java/com/isep/swapcampus/ml/BarcodeHelper.kt`: ML Kit barcode reading calls.
- `shared/src/commonMain/kotlin/com/isep/shared/domain/CategorySuggester.kt`: mapping labels to categories.
- `shared/src/commonMain/kotlin/com/isep/shared/domain/BarcodeText.kt`: adding the reference to the description without duplicate lines.

The ML Kit libraries are bundled in the APK. Detector calls remain in the Android module.

## Verification

The complete version has seven Kotlin rule tests and two instrumented Android reader tests (EAN-13 and an image without a code). These tests do not cover every user interaction.

```sh
./gradlew :app:assembleDebug :shared:testAndroidHostTest :app:testDebugUnitTest
./gradlew :app:connectedDebugAndroidTest
```

The second command needs a connected Android device or emulator. Android Studio can provide Java. `local.properties` is specific to each computer and is not included. `debug.keystore` is only for test builds.

## Classroom exercise

Use the separate `II.3510_2627_Skills_MLkit_UP` folder for the unfinished project. Its `EXERCISE.md` explains the four TODOs. Do not give the complete project to classmates before they have attempted the exercise.
