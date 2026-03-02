# Agents

## Cursor Cloud specific instructions

This is a **Flutter plugin monorepo** ([audioplayers](https://github.com/bluefireteam/audioplayers)) managed by **Melos** with Dart pub workspaces. It provides cross-platform audio playback for Flutter apps.

### Key commands

| Task | Command |
|---|---|
| Bootstrap dependencies | `melos bootstrap` |
| Format check | `melos format --set-exit-if-changed` |
| Static analysis | `flutter analyze --no-fatal-warnings` |
| Run unit tests | `melos run test` |
| Build Linux example app | `cd packages/audioplayers/example && flutter build linux --debug` |
| Start test HTTP server | `cd packages/audioplayers/example/server && dart run bin/server.dart` |
| Run Linux integration tests | See `.github/workflows/test.yml` (`linux` job) |

See `contributing.md` for full contributor setup details and `pubspec.yaml` (root) for melos scripts.

### Non-obvious caveats

- **Xvfb required for headless Linux**: The Flutter Linux app needs a display. Start `Xvfb -ac :99 -screen 0 1280x1024x24 &` and set `DISPLAY=:99` before building or running the Linux example or integration tests.
- **libstdc++-14-dev needed**: On Ubuntu 24.04 with clang-18, you must install `libstdc++-14-dev` to resolve `'type_traits' file not found` build errors. Additionally, create a symlink `sudo ln -sf /usr/lib/gcc/x86_64-linux-gnu/13/libstdc++.so /usr/lib/x86_64-linux-gnu/libstdc++.so` for the linker.
- **Release builds**: `flutter build linux` (release mode) tries to install to `/usr/local/` and may fail with permission errors. Use `flutter build linux --debug` or run release builds with `sudo`.
- **Pre-existing format issue**: `audioplayers/lib/src/audioplayer.dart` has a pre-existing `dart format` issue (line length + trailing comma).
- **GStreamer plugins**: `gstreamer1.0-plugins-good` and `gstreamer1.0-plugins-bad` are needed at runtime for various audio formats (mp3, m3u8, etc.).
- **Test server for integration tests**: Integration tests require the local test server running (`cd packages/audioplayers/example/server && dart run bin/server.dart`) and `--dart-define USE_LOCAL_SERVER=true`.
- **Linux integration tests**: Use `LIVE_MODE=true` for the test server on Linux to avoid flakiness.
