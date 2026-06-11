# Logs

Logging is done using the [android_logger](https://crates.io/crates/android_logger) crate, which is compatible with the [log](https://crates.io/crates/log) crate.

Here is the adb command to get log from the device: `adb logcat | grep your_app`

todo: a tool similar to the logcat panel in Android studio ?
