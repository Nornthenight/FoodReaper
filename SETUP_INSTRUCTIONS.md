# FoodReaper - Complete Project Setup Guide

## Project Info

FoodReaper is an Android food inventory management app built with **Kotlin 2.0 + Jetpack Compose + Material 3 Expressive**.

## Quick Setup

1. Create new **Empty Activity** project in Android Studio (Min SDK 24, Target SDK 35)
2. Copy all files from the structure below into your project
3. Sync Gradle
4. Run on your device!

## Project Structure

```
FoodReaper/
├── app/src/main/
│   ├── kotlin/com/foodreaper/
│   │   ├── MainActivity.kt
│   │   ├── model/Product.kt
│   │   ├── database/AppDatabase.kt
│   │   ├── database/ProductDao.kt
│   │   ├── repository/ProductRepository.kt
│   │   ├── viewmodel/ProductViewModel.kt
│   │   ├── ui/screens/HomeScreen.kt
│   │   ├── ui/screens/AddProductScreen.kt
│   │   ├── ui/components/ProductCard.kt
│   │   ├── ui/theme/Color.kt
│   │   ├── ui/theme/Typography.kt
│   │   ├── ui/theme/Theme.kt
│   │   ├── worker/ExpiryReminderWorker.kt
│   │   └── utils/NotificationUtils.kt
│   ├── AndroidManifest.xml
│   └── res/values/
├── build.gradle.kts (Project)
├── app/build.gradle.kts (Module)
├── settings.gradle.kts
└── .gitignore
```

## Key Files & Their Content

###  build.gradle.kts (Project Level)

```kotlin
plugins {
    id("com.android.application") version "8.1.1" apply false
    id("org.jetbrains.kotlin.android") version "2.0.0" apply false
    id("org.jetbrains.kotlin.plugin.compose") version "2.0.0" apply false
}
```

### app/build.gradle.kts (Module Level)

```kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
    id("org.jetbrains.kotlin.plugin.compose")
    id("kotlin-kapt")
}

android {
    namespace = "com.foodreaper"
    compileSdk = 35
    
    defaultConfig {
        applicationId = "com.foodreaper"
        minSdk = 24
        targetSdk = 35
        versionCode = 1
        versionName = "1.0"
    }
    
    buildTypes {
        release {
            isMinifyEnabled = false
        }
    }
    
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_17
        targetCompatibility = JavaVersion.VERSION_17
    }
    
    kotlinOptions {
        jvmTarget = "17"
    }
    
    buildFeatures {
        compose = true
    }
    
    composeOptions {
        kotlinCompilerExtensionVersion = "1.5.8"
    }
}

dependencies {
    // Compose
    implementation("androidx.compose.ui:ui:1.6.0")
    implementation("androidx.compose.material3:material3:1.1.1")
    implementation("androidx.activity:activity-compose:1.8.0")
    
    // Room Database
    implementation("androidx.room:room-runtime:2.6.0")
    kapt("androidx.room:room-compiler:2.6.0")
    implementation("androidx.room:room-ktx:2.6.0")
    
    // ViewModel
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.6.1")
    
    // WorkManager
    implementation("androidx.work:work-runtime-ktx:2.8.1")
    
    // Testing
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.5.1")
}
```

## Installation Steps

1. **Create New Project**
   - File > New > New Android Project
   - Select "Empty Activity"
   - Name: FoodReaper
   - Package: com.foodreaper
   - Min SDK: 24, Target: 35
   - Language: Kotlin

2. **Update Gradle Files**
   - Copy build.gradle.kts (Project) content
   - Copy app/build.gradle.kts (Module) content

3. **Create Package Structure**
   - `kotlin/com/foodreaper/` (main package)
   - `kotlin/com/foodreaper/model/`
   - `kotlin/com/foodreaper/database/`
   - `kotlin/com/foodreaper/repository/`
   - `kotlin/com/foodreaper/viewmodel/`
   - `kotlin/com/foodreaper/ui/screens/`
   - `kotlin/com/foodreaper/ui/components/`
   - `kotlin/com/foodreaper/ui/theme/`
   - `kotlin/com/foodreaper/worker/`
   - `kotlin/com/foodreaper/utils/`

4. **Sync Gradle**
   - File > Sync Now

5. **Run**
   - Run > Run 'app'

## Key Features

✅ **Inventory Management**: Add products with expiry dates
✅ **Daily Reminders**: WorkManager background notifications
✅ **Material 3 Expressive**: Modern UI with HCT colors
✅ **Room Database**: Local data persistence
✅ **Jetpack Compose**: Modern declarative UI

## Next Steps

- Add barcode scanner (ML Kit)
- Implement recipe generator
- Add Yandex Market integration
- Push to Google Play Store

## Support

For full source code, visit: https://github.com/Nornthenight/FoodReaper

**Happy Coding! 🚀**
