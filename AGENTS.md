# QDomyos-Zwift

Qt application bridging fitness equipment and virtual training platforms.
Use current source and the requested outcome. Resolve routine implementation
choices yourself; ask only when missing intent materially changes the result or
an action exceeds scope. Native T3 owns agent orchestration and settings.

## Source map and contracts

- `src/devices/`: device types derive from `bluetoothdevice` through bike,
  treadmill, elliptical, rower and related bases. Detection order in
  `src/devices/bluetooth.cpp` is first-match: test intended names and overlapping
  prefixes, keeping specific patterns before broad ones.
- Add devices to `src/qdomyos-zwift.pri` and the existing `tst/Devices` fixtures.
  [Testing](docs/50_writing_tests.md) owns build/test mechanics.
- `src/virtualdevices/` owns virtual equipment; `src/characteristics/` owns
  Bluetooth protocols. Reuse matching implementations and source specifications.
- Settings/QML changes follow [Settings Catalog](docs/settings-catalog.md).
  Preserve property order and synchronized QZSettings/catalog declarations.
  Run existing `tools/check_settings_catalog.py BASE_REF` and
  `tools/check_settings_property_order.py BASE_REF` for affected changes.
- [ProForm capture](docs/proform-device-support.md) contains protocol-specific
  integration facts; load it only for those devices.

## Build and platform checks

Use the affected platform's current recipe in `.github/workflows/main.yml` and
existing Qt tools. Device fixtures require the built application library.
[Android testing](docs/android-testing/README.md) owns native/emulator checks;
[Waydroid notes](docs/android-testing/waydroid-build-notes.md) preserve historical
packaging/rendering pitfalls, not facts about the current machine.
Documentation-only changes need link/meaning checks, not platform builds or
live Bluetooth/account access. Run broader tests only for a changed risk.

For releases, keep `src/android/AndroidManifest.xml` versionName/versionCode,
`src/main.qml` displayed version, and `src/qdomyos-zwift.pri` VERSION aligned;
Android build numbers increase. iOS versions belong to its Xcode project.
Keep credentials and real-account captures out of source and diagnostics.
