# Build Gaming DNS APK

## GitHub Actions (recommended)

This repository intentionally keeps generated Android Gradle files out of the source bundle. The workflow generates a fresh Android scaffold using the current Flutter stable channel, which supplies `gradlew`, `gradlew.bat`, and `gradle-wrapper.jar`, then restores the custom manifest, Kotlin VPN service, method channel, and app theme. It supports both Groovy (`settings.gradle` / `build.gradle`) and Kotlin DSL (`settings.gradle.kts` / `build.gradle.kts`) templates. This avoids local/CI failures caused by a missing Gradle Wrapper.

1. Create a GitHub repository.
2. Upload the **contents** of this folder to the repository root and push to `main` or `master`.
3. Open **Actions → Build Gaming DNS APK**.
4. Review the `Run IP converter unit tests` step. The IPv4/IPv6 converter unit-test step must pass. The Dart analyzer step is informational; the release build itself must succeed.
5. Download `Gaming-DNS-Release-APK` from the run's **Artifacts** section.

If the run fails, download the `Gaming-DNS-Build-Logs` artifact; it includes the available scaffold, dependency, test, analyzer, and build logs. If needed, also open the first failed step and share its complete log. The precise failure matters: dependency resolution, Dart compilation, Kotlin compilation, and Android manifest errors need different fixes.

## Local build

Install the current Flutter stable SDK, Android SDK, and Java 17. Since the bundle does not include generated Gradle Wrapper files, do not run `flutter build apk` before preparing the Android platform.

From PowerShell in the project root:

```powershell
flutter create --platforms=android --org com.example --project-name gaming_dns .
flutter pub get
flutter test test/ip_converter_test.dart
flutter build apk --release
```

**Important:** `flutter create` can replace Android files. Restore these project-specific files afterwards if they were overwritten:

- `android/app/src/main/AndroidManifest.xml`
- `android/app/src/main/kotlin/com/example/gaming_dns/MainActivity.kt`
- `android/app/src/main/kotlin/com/example/gaming_dns/GamingVpnService.kt`
- `android/app/src/main/res/drawable/ic_launcher.xml`
- `android/app/src/main/res/values/styles.xml`
- `android/app/src/main/res/values/colors.xml`

For repeatable local preparation, use the GitHub Actions workflow as the reference implementation. The workflow output is a debug-signed release APK for testing; store distribution requires a private release keystore and signing configuration. Never commit the keystore or passwords to a public repository.

## IPv4/IPv6 conversion notes

- IPv4 input produces an IPv4-mapped IPv6 representation (`::ffff:a.b.c.d`) and an IPv6 address using the well-known NAT64 `/96` prefix (`64:ff9b::hhhh:hhhh`).
- IPv4 can be recovered only from those recognized representations.
- A native IPv6 address has no universal corresponding IPv4 address. The app does not fabricate one.
- Generated address formats are not proof that a DNS resolver is listening or reachable at those addresses.
