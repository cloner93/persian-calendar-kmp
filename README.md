# Persian Calendar - Kotlin Multiplatform

[![](https://jitpack.io/v/persian-calendar/calendar.svg)](https://jitpack.io/#persian-calendar/calendar)

A **Kotlin Multiplatform** library for Persian (Jalali), Islamic (Hijri) and Gregorian calendar conversions.

## 🌟 Features

-  **Pure Kotlin** - No platform dependencies
-  **Multiplatform** - Android, iOS, JVM, JS, WASM
-  **Lightweight** - Zero external dependencies
-  **Accurate** - Based on astronomical calculations
-  **Fast** - Optimized lookup tables where possible
-  **Well-tested** - Comprehensive test coverage

## 📦 Installation
### Current Stable Version (Android / JVM)
Use the latest released version (1.4.0):

**Android**
```kotlin
// settings.gradle.kts
repositories {
    maven("https://jitpack.io")
}

// build.gradle.kts
dependencies {
    implementation("com.github.persian-calendar:calendar:1.4.0")
}
```

### Kotlin Multiplatform (Upcoming Feature)
KMP support is in development (see PR #78) and will be available in a future release (likely 1.5.0+).Once released, you can use it like this:

```kotlin
// settings.gradle.kts
repositories {
    maven("https://jitpack.io")
}

// build.gradle.kts
kotlin {
    sourceSets {
        commonMain.dependencies {
            implementation("com.github.persian-calendar:calendar:1.5.0") // or later
        }
    }
}
```
For now, if you want to test the KMP changes early:

Clone the repository and build locally: 
```gradle
./gradlew publishToMavenLocal
```
Then depend on "com.github.persian-calendar:calendar:unspecified" (or the version from local Maven).

## 🚀 Quick Start

### Basic Usage

```kotlin
import io.github.persiancalendar.calendar.*

// Create a Persian date
val persian = PersianDate(1403, 11, 12)

// Convert to Gregorian
val gregorian = CivilDate(persian)
println(gregorian) // CivilDate(2025, 2, 1)

// Convert to Islamic
val islamic = IslamicDate(persian)
println(islamic) // IslamicDate(1446, 7, 21)

// Use Julian Day Number for conversions
val jdn = persian.toJdn()
val backToPersian = PersianDate(jdn)
```

### Working with Components

```kotlin
// Destructuring
val (year, month, day) = PersianDate(1403, 1, 1)

// Month operations
val nextMonth = persian.monthStartOfMonthsDistance(1)
val distance = persian.monthsDistanceTo(PersianDate(1404, 1, 1))
```

### Supported Calendars

| Calendar | Class | Example |
|----------|-------|---------|
| Persian (Jalali) | `PersianDate` | `PersianDate(1403, 11, 12)` |
| Gregorian | `CivilDate` | `CivilDate(2025, 2, 1)` |
| Islamic (Hijri) | `IslamicDate` | `IslamicDate(1446, 7, 21)` |

### Islamic Calendar Variants

```kotlin
// Use Umm al-Qura calendar (Saudi Arabia)
IslamicDate.useUmmAlQura = true

// Apply offset to Islamic dates
IslamicDate.islamicOffset = -1
```

## 📱 Platform Examples

### Android with Jetpack Compose

```kotlin
@Composable
fun PersianDateDisplay() {
    val today = remember {
        val now = LocalDate.now()
        PersianDate(CivilDate(now.year, now.monthValue, now.dayOfMonth))
    }

    Text("امروز: ${today.year}/${today.month}/${today.dayOfMonth}")
}
```

### iOS with SwiftUI

```swift
import PersianCalendar

struct ContentView: View {
    let persianToday: PersianDate

    init() {
        let now = Date()
        let calendar = Calendar.current
        let components = calendar.dateComponents([.year, .month, .day], from: now)

        let civil = CivilDate(
            year: Int32(components.year!),
            month: Int32(components.month!),
            dayOfMonth: Int32(components.day!)
        )

        persianToday = PersianDate(date: civil)
    }

    var body: some View {
        Text("امروز: \(persianToday.year)/\(persianToday.month)/\(persianToday.dayOfMonth)")
    }
}
```

### Compose Multiplatform

```kotlin
@Composable
fun CalendarApp() {
    var selectedDate by remember { mutableStateOf(PersianDate(1403, 1, 1)) }

    Column {
        Text("Persian: ${selectedDate.year}/${selectedDate.month}/${selectedDate.dayOfMonth}")

        val civil = CivilDate(selectedDate)
        Text("Gregorian: ${civil.year}/${civil.month}/${civil.dayOfMonth}")

        val islamic = IslamicDate(selectedDate)
        Text("Islamic: ${islamic.year}/${islamic.month}/${islamic.dayOfMonth}")
    }
}
```

## 🔧 Advanced Features

### Leap Year Detection

```kotlin
// Persian calendar leap year check (via JDN conversion)
val isPersianLeap = PersianDate(1403, 12, 30).let { date ->
    try {
        date.toJdn()
        true
    } catch (e: Exception) {
        false
    }
}
```

### Date Arithmetic

```kotlin
// Add months
val nextYear = persianDate.monthStartOfMonthsDistance(12)

// Calculate duration
val monthsDiff = startDate.monthsDistanceTo(endDate)
```

### JDN Conversions

```kotlin
// All calendars use Julian Day Number as the universal converter
val jdn = 2460676L

val persian = PersianDate(jdn)
val civil = CivilDate(jdn)
val islamic = IslamicDate(jdn)
```

## 🏗️ Architecture

```
Calendar System
├── AbstractDate (base class)
│   ├── toJdn(): Long (convert to Julian Day Number)
│   └── fromJdn(jdn): DateTriplet (convert from JDN)
│
├── PersianDate (astronomical + lookup tables)
├── CivilDate (Gregorian/Julian hybrid)
└── IslamicDate (multiple calculation methods)
```

## 🧪 Testing

```bash
# Run all tests
./gradlew allTests

./gradlew kotest
```

## 📄 License

```
Copyright 2024 Persian Calendar Contributors

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0
```
