# Android Build Setup

Android application are not standalone binaries. Instead, they are packaged as libraries that are loaded at runtime by the Android system. This is why we compile our Rust crate as a `cdylib`.

The main entry point is the `android_main` function, which receives an `AndroidApp` parameter provided by the `android_activity` crate. This parameter is required by [winit](https://docs.rs/winit/latest/winit/), the windowing library used by [Iced](https://github.com/iced-rs/iced).

To build our package, we can use the official Rust toolchain, with thes es targets:

- `aarch64-linux-android` (64-bit ARM, used by most modern devices)
- `armv7-linux-androideabi` (32-bit ARM, more supported)
- `x86_64-linux-android` (for Android emulators)

During the linking phase, we must use the Android NDK linkers.

For this, you can either

- set this env variable `CARGO_TARGET_AARCH64_LINUX_ANDROID_LINKER="aarch64-linux-android24-clang"`
- or in `.cargo/config.toml`
  ```toml
  [target.aarch64-linux-android]
  linker = "aarch64-linux-android24-clang"
  ```

24 is the `minSdk`.
Either way, you need to have the binary accessible in the PATH. Usually in `/home/user/Android/Sdk/ndk/27.3.13750724/toolchains/llvm/prebuilt/linux-x86_64/bin`.

To avoid dealing with the NDK setup manually, you can use [`cargo ndk`](https://github.com/bbqsrc/cargo-ndk).

The next step is building the APK. The standard approach is to use Gradle.

Alternatively, you can use [cargo apk](https://github.com/rust-mobile/cargo-apk) (or the maintained [cargo apk2](https://github.com/mzdk100/cargo-apk2)) to build and package the application without using Gradle.

Since we will probably don't want to use Android Studio, here are some useful [adb](developer.android.com/tools/adb) commands

- install the app

  ```sh
  adb install -r path/to/apk
  ```

- launch the app
  ```sh
  adb shell monkey \
          -p io.github.wiiznokes.todolist \
          -c android.intent.category.LAUNCHER \
          1
  ```

Since packaging the apk and installing on the device will take time, to iterate faster, you can build the app for you local environement. Add `rlib` to `crate-type` in `Cargo.toml`, and add a `main.rs`.
