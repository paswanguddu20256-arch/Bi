# SmoothOS — High-Performance Android App

SmoothOS is a modern, high-performance Android application built with native Kotlin, Jetpack Compose, and Material 3. Designed specifically around low memory consumption, zero UI thread freezing (anti-jank), resilient multi-network connectivity, and two-tier caching.

---

## Key Features

1. **Anti-Freeze & Zero-Jank Architecture**
   - **Strict Thread Segregation:** All disk I/O, cache serialization, and network calls execute exclusively on `Dispatchers.IO` and `Dispatchers.Default`. The Main UI thread is reserved purely for rendering.
   - **Lifecycle-Aware State Collection:** UI observes state with `collectAsStateWithLifecycle()`, automatically suspending polling and expensive operations when the screen is backgrounded.
   - **Optimized Compose Recycler:** Stable keys on all LazyColumn items with precalculated layout dimensions to maintain 60/120 FPS even under heavy data lists.
   - **In-Memory LRU Cache with Pruning:** Observable cache that tracks hit/miss telemetry and safely evicts aged data when memory pressure occurs.

2. **Real-Time Connectivity Telemetry**
   - **Hardware Connectivity Callback:** Integrates Android's `ConnectivityManager.NetworkCallback` with Kotlin `callbackFlow`.
   - **Deep Network Inspection:** Continuously tracks Wi-Fi, Mobile Data, VPN, Metered status, and real link downstream/upstream bandwidth.
   - **Never Claims Fake Connection:** Validates real internet routability (`NET_CAPABILITY_VALIDATED`) before declaring online status.
   - **Animated Offline Degradation:** If network drops or is simulated offline, SmoothOS smoothly serves cached data with zero crashes or ANRs.

3. **HTTPS Edge Latency Benchmarking**
   - Tests round-trip latency (DNS resolution + TLS 1.3 handshake + transfer time) against public endpoints (Cloudflare 1.1.1.1, Google Gateway, HTTPBin).
   - Canvas-rendered real-time latency bar chart with 100ms baseline.

4. **10,000-Item Zero-Jank Stress Test**
   - Demonstrates instant instantiation, filtering, and smooth scrolling of 10,000 complex items without frame drops or heap fragmentation.

5. **Modern Material 3 Interface**
   - Dynamic Dark and Light themes with high-tech cyan and deep slate aesthetics.
   - Minimum 48dp touch targets and accessibility semantics.
   - Status badge in top app bar for glanceable network health.

---

## Project Structure

```
SmoothOS/
├── .github/workflows/
│   └── android-apk.yml               # Automated CI workflow building tests & APK artifact
├── app/
│   ├── src/main/
│   │   ├── AndroidManifest.xml       # Declares INTERNET & ACCESS_NETWORK_STATE
│   │   ├── java/com/example/
│   │   │   ├── MainActivity.kt       # Edge-to-edge entry point
│   │   │   ├── data/
│   │   │   │   ├── model/            # NetworkStatus, SystemMetrics, DiagnosticsResult, FeedItem
│   │   │   │   ├── network/          # NetworkMonitor, ResilientHttpClient, RetryPolicy, NetworkResult
│   │   │   │   ├── cache/            # LruMemoryCache with observable telemetry
│   │   │   │   ├── system/           # PerformanceTelemetryCollector (Heap, RAM, Threads, Battery)
│   │   │   │   └── repository/       # SmoothRepository
│   │   │   └── ui/
│   │   │       ├── SmoothOSApp.kt    # Root Scaffold, top bar, navigation bar
│   │   │       ├── components/       # NetworkStatusBadge, OfflineBanner, MetricCard, LatencyBarChart
│   │   │       ├── screens/          # DashboardScreen, DiagnosticsScreen, SettingsScreen
│   │   │       ├── theme/            # Color, Theme, Type
│   │   │       └── viewmodel/        # SmoothViewModel
│   │   └── res/                      # Drawables, strings, adaptive launcher icons
│   └── src/test/java/com/example/    # JVM unit & Robolectric tests
├── build.gradle.kts
└── settings.gradle.kts
```

---

## How to Build and Run

### Option 1: Automated Build via GitHub Actions (Recommended for Mobile/Cloud)

1. Push your repository to GitHub.
2. In your browser or GitHub mobile app, navigate to the **Actions** tab.
3. Select the **SmoothOS Android APK Build & Test** workflow.
4. Tap **Run workflow**.
5. Once complete (usually 1-2 minutes), click on the workflow run.
6. Under **Artifacts**, tap **SmoothOS-Debug-APK** to download your compiled `.apk` file.

### Option 2: Local Android Studio

1. Open Android Studio (Ladybug / Iguana or later).
2. Choose **File > Open** and select the root directory of this project.
3. Allow Gradle to synchronize dependencies.
4. Connect an Android phone via USB (with Developer Options and USB Debugging enabled) or start an Android Emulator.
5. Click the green **Run (Play)** button or press `Shift + F10`.

### Option 3: Command Line (Gradle)

```bash
# Run unit tests and Robolectric JVM tests
gradle :app:testDebugUnitTest

# Compile the debug APK
gradle :app:assembleDebug
```
The output APK is located at:
`app/build/outputs/apk/debug/app-debug.apk`

---

## Installing the APK on an Android Phone

1. Transfer or download the generated `app-debug.apk` onto your Android phone (via Google Drive, USB cable, or directly downloading the GitHub artifact).
2. Open your phone's **Files** or **Downloads** app and tap `app-debug.apk`.
3. If Android displays:
   > *"For your security, your phone is not allowed to install unknown apps from this source."*
   - Tap **Settings**.
   - Enable **Allow from this source**.
   - Tap **Back** and select **Install**.
4. Launch **SmoothOS** from your phone's home screen or app drawer.

---

## Publishing to Google Play Store

Publishing to Google Play requires production packaging:

1. **Android App Bundle (AAB):** Google Play requires an `.aab` format rather than `.apk`. Build it using:
   ```bash
   gradle :app:bundleRelease
   ```
2. **Release Signing Keystore:** Create a private upload keystore with `keytool`:
   ```bash
   keytool -genkey -v -keystore my-upload-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
   ```
   *Never commit `.jks` files or passwords into GitHub repository code.*
3. **Google Play Console:** Upload the `.aab` file to your Google Play Console developer account under Production or Internal Testing track.

---

## Security & Privacy Compliance

- **No Secrets Embedded:** SmoothOS does not hardcode passwords, API secrets, or personal tokens.
- **Least-Privilege Permissions:** Uses only `INTERNET` and `ACCESS_NETWORK_STATE` to detect network status.
- **Data Protection:** All remote diagnostics use HTTPS with TLS 1.3 encryption. No user personal data is gathered or transmitted.
