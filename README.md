# Lut's C

**Lut's C** is a Flutter reading sanctuary for your own PDF, EPUB, and TXT books. It pairs a warm desert palette with frosted liquid-glass surfaces, custom vintage collage covers, a multi-book reading stack, and an in-reader Arabic ⇄ English translation tool.

## What is included

- First-launch glass language gate (Arabic / English) with full RTL / LTR mirroring.
- Four-tab floating glass navigation: Home, My Library, Reader, Settings.
- Local-only imports from device storage. Files are copied into the app's documents directory.
- A cover studio with six engraved collage figures (Plato, Machiavelli, Mona Lisa, Cleopatra, Ibn Khaldun, Napoleon) and five desert paper palettes.
- Persistent library, cover choices, typography, theme, blur strength, reading progress, and the active multi-book stack.
- A high-resolution session overview from the Reader tab (tap or long-press) showing every open book at its current page — the real PDF page, or a paper excerpt for text books.
- Continuous, selectable text reading with frosted edge blurs, plus a page-by-page PDF canvas.
- Arabic ⇄ English translation for selected text (text books) or the current page (PDFs). Translation requires an internet connection and uses a free public translation endpoint.

## Run

```bash
flutter pub get
flutter run
```

Platform folders for **Android**, **iOS**, and **web** are committed, so no `flutter create` step is needed.

Current target: Flutter 3.47 stable (validated on 3.47.7 / Dart 3.13.5).

### Android

No extra permissions are required: imports use the system document picker. PDF rendering is bundled through `pdfrx` / PDFium.

### iOS

The document picker needs no Info.plist keys for importing. `Lut’s C` is already set as the display name.

### Desktop

`pdfrx` requires Windows Developer Mode for its symlink-based native PDFium linking. Desktop folders are not committed; add one with `flutter create --platforms=windows .` (Visual Studio with the C++ workload is required).

### Web

The project compiles for the web (`flutter build web`), but the reader is built around local files: browser runtimes need a different storage backend before the library and importer work there.

## Verification

```bash
flutter analyze   # clean
flutter test      # EPUB parsing, formatters, script detection, book persistence
```

## Project shape

```
lib/
  core/       theme tokens, typography, localization, shared utilities
  data/       models, storage, import parsers, EPUB extraction, translation
  state/      Provider controllers for settings, library, and reading sessions
  ui/         shell, onboarding, home, library, reader, sessions, settings, import
assets/fonts/ Cairo (variable), Amiri (regular + bold), Reem Kufi (variable)
test/         pure-logic tests for the parser, formatters, and models
```

## Notes

- No sample books, placeholder covers, or demo metadata are inserted. The library is intentionally empty until you import a file; covers are drawn entirely in code with `CustomPainter`.
- Imported files never leave the device. Only the text you explicitly send to the translation tool is transmitted to the translation service.
- Translation is a convenience layer around public endpoints (`translate.googleapis.com` with a MyMemory fallback); for production distribution, point `TranslationService` at a managed translation API.
- Reading progress is stored per book and per session, so switching between several open books resumes each one exactly where it was left.
