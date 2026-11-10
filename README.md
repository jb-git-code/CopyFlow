<div align="center">

# 📋 copyflow

### A background clipboard manager for Android — built with Flutter

Capture, view, and reuse your clipboard history even when the app isn't open.

[![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)](#)
[![Flutter](https://img.shields.io/badge/Built%20with-Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)](#)
[![Kotlin](https://img.shields.io/badge/Native-Kotlin-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)](#)
[![Open Source](https://img.shields.io/badge/Open%20Source-💙-blue?style=for-the-badge)](#)

</div>

---

## ✨ What is copyflow?

copyflow is an open-source Android app that keeps a running history of everything you copy — across **every app on your phone** — not just the one item Android normally remembers. Copy something in Chrome, WhatsApp, Notes, or anywhere else, and it shows up instantly in copyflow, ready to be reused with a single tap.

Everything stays **on your device**. Nothing is uploaded anywhere.

> 🧠 Built as a learning project to solve a genuinely hard Android platform problem: reading the clipboard reliably from the background, something Android intentionally restricts since Android 10.

---

## 🚀 Features

- 📡 **Live background capture** — detects clipboard changes even while the app is minimized
- 🕘 **Full clipboard history** — not just the last item, a scrollable list of everything you've copied
- 🔁 **One-tap re-copy** — tap any card to instantly push it back to the system clipboard
- 🔒 **100% local** — no network calls, no accounts, no cloud sync
- 🪶 **Lightweight** — no ads, no telemetry, no bloat

---

## 🛠️ How it works

Android intentionally blocks apps from reading the clipboard unless they're currently in focus (since Android 10), to prevent silent clipboard-snooping by malicious apps. copyflow works around this the same way real-world open-source clipboard managers do:

```
┌──────────────────────────┐
│   AccessibilityService   │  Watches for taps / text-selection
│   (native, Kotlin)       │  events across the whole system
└────────────┬─────────────┘
             │ triggers
             ▼
┌──────────────────────────┐
│    Invisible Overlay     │  Briefly grabs system focus using
│    Window (native)       │  a 1x1px transparent window
└────────────┬─────────────┘
             │ reads
             ▼
┌──────────────────────────┐
│   ClipboardManager       │  Reads the actual clipboard content
│   (Android system API)   │  while focus is held
└────────────┬─────────────┘
             │ streams via EventChannel
             ▼
┌──────────────────────────┐
│        Flutter UI        │  Displays live clipboard history
└──────────────────────────┘
```

This requires two permissions, both explained in detail in-app before you grant them:

|         Permission          |                          Why it's needed                           |
|-----------------------------|--------------------------------------------------------------------|
|  **Accessibility Service**  | Detects when a copy action likely occurred, anywhere on the device |
| **Display over other apps** | Lets the app briefly gain focus to legally read clipboard content  |

See [`PRIVACY.md`](./PRIVACY.md) for exactly what data this touches and where it goes (spoiler: nowhere but your device).

---

## 📲 Installation

### Option 1 — Download a prebuilt APK
Grab the latest APK from the [Releases](../../releases) page and sideload it.

> ⚠️ Android/Play Protect may flag this app on first install — this is expected for any app combining Accessibility + overlay permissions (the same pattern used by clipboard-stealing malware, even though this app does the opposite). The source is fully open here for you to verify. See [`PRIVACY.md`](./PRIVACY.md) for details.

### Option 2 — Build it yourself (APK)

```bash
git clone https://github.com/jb-git-code/copyflow.git
cd copyflow
flutter pub get
flutter build apk --release
```

The built APK will be at `build/app/outputs/flutter-apk/app-release.apk`.

### Option 3 — Build & run directly from an IDE (recommended, most reliable)

This is the most guaranteed way to get a working install, since it skips sideloading and Play Protect warnings entirely.

1. Open the project in Android Studio or VS Code (with the Flutter/Dart plugins installed).
2. Enable **Developer Options → USB Debugging** on your Android phone.
3. Connect your phone to your computer via USB and authorize the connection when prompted.
4. Run:

```bash
flutter devices   # confirm your phone is detected
flutter run       # builds, installs, and launches the app on your phone
```

This builds the app straight onto your connected device — no APK file to download or sideload, and no security prompts to bypass.

---

## 🔐 Permissions setup (first launch)

1. Open the app — it'll show a permission screen with two toggles.
2. Tap **Accessibility Service** → enable *copyflow* in the system list.
3. Tap **Display over other apps** → enable the toggle for *copyflow*.
4. Return to the app — both should show ✅, and you're ready to go.

---

## 🧩 Tech Stack

- **Flutter** — cross-platform UI
- **Kotlin** — native Android AccessibilityService + overlay logic
- **MethodChannel / EventChannel** — native ↔ Flutter bridge for permissions and live clipboard streaming

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repo
2. Create a branch (`git checkout -b feature/your-feature`)
3. Commit your changes
4. Open a pull request

If you're fixing a clipboard-detection edge case (some apps show "Copy" as an icon, others as text, others via keyboard shortcuts), please describe which app/scenario you tested — this logic is inherently heuristic and every real-world test case helps.

---

## ⚠️ Known limitations

- Some OEMs (Xiaomi, Vivo, Oppo, etc.) aggressively kill background Accessibility Services to save battery — users may need to disable battery optimization for the app manually.
- Detection is heuristic, not guaranteed — extremely fast or unusual copy interactions in some apps may occasionally be missed.
- Not published on the Google Play Store due to Play's Accessibility API policy restrictions for non-disability use cases — see [`PRIVACY.md`](./PRIVACY.md) and the project discussion for context.

---

## 📄 License

License to be added by the project owner.

---

## 🙋 Author

Built by **Jayanshu Bhardwaj** — [@jb-git-code](https://github.com/jb-git-code)

<div align="center">

Made with ☕ and a lot of trial and error against Android's clipboard restrictions.

</div>