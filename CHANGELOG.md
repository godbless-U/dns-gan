# Changelog

## 2.2.2 - Flutter Android scaffold fix

- Fixed GitHub Actions failure when Flutter stable generates `settings.gradle.kts` instead of `settings.gradle`.
- Added a Gradle patch script that detects both Groovy and Kotlin DSL Android files.
- Updated the workflow to select either `settings.gradle[.kts]` and `app/build.gradle[.kts]` safely.
- Aligned Gradle wrapper versions with the detected Android Gradle Plugin when required.
- Saved Flutter scaffold, dependency, unit-test, analyzer, and release-build logs as artifacts when a build fails.
- Bumped app version to 2.2.2+6.


## 2.2.1 - Build compatibility fix

- Removed the deleted `withData` argument from `FilePicker.platform.pickFiles`; file_picker 13.x no longer accepts it.
- Updated `share_plus` minimum to 13.3.1 and declared Dart 3.10+ to match dependency requirements.
- Aligned Android Gradle Plugin, Gradle wrapper and Kotlin versions with share_plus 13.x requirements.
- Added build log artifacts for failed GitHub Actions runs.
- Kept IPv4-mapped IPv6 and NAT64 conversion clearly labelled as address representations, not as proof that a resolver exists at the generated address.
