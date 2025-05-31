# Google Ad Manager Interstitial Ads Integration (Jetpack Compose)

This guide provides step-by-step instructions to create an Android project using Jetpack Compose and integrate Google Ad Manager (AdMob) interstitial ads.

## 1. Create a New Android Project

1. Open **Android Studio**.
2. Click on **New Project**.
3. Select **Empty Activity** and click **Next**.
4. Name your project (e.g., `GoogleAdsExample`).
5. Choose **Kotlin** as the language.
6. Select **Minimum SDK** (e.g., API 28 or higher).
7. Click **Finish**.

## 2. Add Google Mobile Ads SDK Dependency

1. Open your project's `gradle/libs.versions.toml` and add:
    ```toml
    googleAds = "23.1.0"
    google-mobile-ads = { group = "com.google.android.gms", name = "play-services-ads", version = "googleAds" }
    google-mobile-ads-interstitial = { group = "com.google.android.gms", name = "play-services-ads-lite", version = "googleAds" }
    ```
2. In your app module's `build.gradle.kts`, add:
    ```kotlin
    implementation(libs.google.mobile.ads)
    implementation(libs.google.mobile.ads.interstitial)
    ```

## 3. Configure AndroidManifest.xml

1. Add the following permissions and meta-data inside the `<manifest>` and `<application>` tags:
    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="com.google.android.gms.permission.AD_ID"/>

    <application
        ... >
        <meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="ca-app-pub-3940256099942544~3347511713"/>
        ...
    </application>
    ```
    - Replace the value with your real AdMob App ID for production.

## 4. Initialize the Mobile Ads SDK

In your `MainActivity.kt`, initialize the Mobile Ads SDK in `onCreate`:
```kotlin
MobileAds.initialize(this) { }
```

## 5. Load and Display Interstitial Ads (Jetpack Compose)

1. Use Compose state and effects to load and show interstitial ads.
2. Example composable:
    ```kotlin
    @Composable
    fun InterstitialAdDemo(modifier: Modifier = Modifier) {
        var interstitialAd by remember { mutableStateOf<InterstitialAd?>(null) }
        var adLoaded by remember { mutableStateOf(false) }
        val context = LocalContext.current

        LaunchedEffect(Unit) {
            val adRequest = AdRequest.Builder().build()
            InterstitialAd.load(
                context,
                "ca-app-pub-3940256099942544/1033173712", // Test Ad Unit ID
                adRequest,
                object : InterstitialAdLoadCallback() {
                    override fun onAdLoaded(ad: InterstitialAd) {
                        interstitialAd = ad
                        adLoaded = true
                    }
                    override fun onAdFailedToLoad(error: LoadAdError) {
                        interstitialAd = null
                        adLoaded = false
                    }
                }
            )
        }

        Column(modifier = modifier) {
            Button(onClick = {
                interstitialAd?.show(context as Activity)
                adLoaded = false
            }, enabled = adLoaded) {
                Text("Show Interstitial Ad")
            }
        }
    }
    ```

## 6. Notes

- Use the provided **test ad unit IDs** for development. Replace with your own IDs for production.
- Do not use both `play-services-ads` and `play-services-ads-lite` unless required. Usually, only `play-services-ads` is needed.
- Always follow [Google’s AdMob policies](https://support.google.com/admob/answer/6128543?hl=en).

---

**You now have a Jetpack Compose Android app with Google Ad Manager interstitial ads integration!**

