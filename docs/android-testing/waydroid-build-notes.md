# Historical Waydroid / Qt Android pitfalls

These observations came from the former root AGENTS instructions and apply to
the recorded Qt 5.15 Android/NDK 21.3 setup. Inspect the actual toolchain/device
before applying them; paths and host state are intentionally not prescribed.

- The recorded qmake Makefile aggregated all four ABI targets even when
  `ANDROID_ABIS` was set. A specific target such as `make x86_64-all` selected
  that ABI; `make install` could still rebuild all four. Read the device ABI
  with `adb shell getprop ro.product.cpu.abi`.
- Fresh worktrees lacked untracked `src/translations/*.qm`. Generate translations
  using the repository scripts or reuse matching verified artifacts before
  resource compilation.
- `androiddeployqt --aux-mode --no-build` sometimes omitted Qt runtime `.so`
  files or `assets/android_rcc_bundle.rcc`. Missing `.so` files caused
  `UnsatisfiedLinkError`; a missing QML resource bundle yielded empty root
  objects and a later `homeform`/`QList::first()` assertion. Inspect the package;
  reuse artifacts only from a verified build with the same Qt/ABI identity.
- The recorded AGP 8.13 debug build required Java 17. Current Gradle/AGP source
  owns the current Java requirement.
- Waydroid's Bluetooth scan could remain unfinished, so fake-device activation
  in `bluetooth::finished()` never ran. This did not establish a physical-device
  defect. `adb shell run-as` also failed with `setegid(AID_PACKAGE_INFO) failed`;
  settings had to be exercised through the UI.
- The recorded QtWebView/Chart.js surface froze visually while JavaScript data,
  pixel coordinates and `chart.update()` continued changing. Both screenshots
  and recordings showed the stale frame; touch temporarily forced repaint.
  Separate instrumented data behavior from physical-device visual acceptance.
- `myMessageOutput` in `src/main.cpp` suppressed QML warnings unless the
  `log_debug` setting was enabled. Diagnose with the existing logging control
  and remove temporary instrumentation after the relevant check.

[Android testing](README.md) owns current procedures. None of these observations
authorizes device/account changes or proves the current host is configured.
