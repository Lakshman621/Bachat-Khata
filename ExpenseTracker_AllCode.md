# Daily Expense Tracker – ALL CODE (one file)

Each section header is the file path. Create the file at that path and paste the code.
Package for all Kotlin files: `com.example.expense` (folder `app/src/main/java/com/example/expense/`).

Order: Gradle → Manifest → Resources (values, drawables, menu) → Kotlin → Layouts.

---

# PART 1 – GRADLE

### `settings.gradle.kts`
```kotlin
pluginManagement {
    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
    }
}
rootProject.name = "DailyExpenseTracker"
include(":app")
```

### `build.gradle.kts` (project root)
```kotlin
plugins {
    id("com.android.application") version "8.5.2" apply false
    id("org.jetbrains.kotlin.android") version "1.9.24" apply false
    id("com.google.devtools.ksp") version "1.9.24-1.0.20" apply false
}
```

### `app/build.gradle.kts`
```kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
    id("com.google.devtools.ksp")
}

android {
    namespace = "com.example.expense"
    compileSdk = 34

    defaultConfig {
        applicationId = "com.example.expense"
        minSdk = 24
        targetSdk = 34
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
        viewBinding = true   // gives us generated classes like FragmentHomeBinding
    }
}

dependencies {
    implementation("androidx.core:core-ktx:1.13.1")
    implementation("androidx.appcompat:appcompat:1.7.0")
    implementation("com.google.android.material:material:1.12.0")
    implementation("androidx.recyclerview:recyclerview:1.3.2")
    implementation("androidx.fragment:fragment-ktx:1.8.2")
    implementation("androidx.activity:activity-ktx:1.9.1")

    // MVVM (ViewModel + LiveData)
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.8.4")
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.8.4")

    // Room database
    implementation("androidx.room:room-runtime:2.6.1")
    implementation("androidx.room:room-ktx:2.6.1")
    ksp("androidx.room:room-compiler:2.6.1")
}
```

---

# PART 2 – MANIFEST

### `app/src/main/AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- No permissions needed: the app is fully offline -->
    <application
        android:name=".ExpenseApp"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.ExpenseTracker">

        <activity
            android:name=".ui.SplashActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".ui.MainActivity" android:exported="false" />

        <activity
            android:name=".ui.AddTransactionActivity"
            android:exported="false"
            android:windowSoftInputMode="adjustResize" />
    </application>
</manifest>
```

---

# PART 3 – RESOURCES

### `app/src/main/res/values/strings.xml`
```xml
<resources>
    <string name="app_name">Expense Tracker</string>
</resources>
```

### `app/src/main/res/values/colors.xml`
```xml
<resources>
    <color name="white">#FFFFFFFF</color>
    <color name="primary">#5B4BDB</color>
    <color name="primary_dark">#4435B5</color>
    <color name="primary_container">#E6E3FB</color>
    <color name="on_primary">#FFFFFF</color>
    <color name="secondary">#3F8CFF</color>
    <color name="bg">#F4F5FB</color>
    <color name="card">#FFFFFF</color>
    <color name="text_primary">#1B1B2F</color>
    <color name="text_secondary">#6B6F80</color>
    <color name="income">#1FA971</color>
    <color name="expense">#E5484D</color>
    <color name="warning">#F5A524</color>
    <color name="divider">#E3E5EE</color>
    <color name="gradient_start">#5B4BDB</color>
    <color name="gradient_end">#3F8CFF</color>
</resources>
```

### `app/src/main/res/values-night/colors.xml`
```xml
<resources>
    <color name="primary">#8F83FF</color>
    <color name="primary_container">#2E2A5A</color>
    <color name="on_primary">#14122B</color>
    <color name="bg">#12121C</color>
    <color name="card">#1E1E2E</color>
    <color name="text_primary">#F1F1F7</color>
    <color name="text_secondary">#A0A3B5</color>
    <color name="income">#3DD598</color>
    <color name="expense">#FF6B6B</color>
    <color name="divider">#2E2F42</color>
</resources>
```

### `app/src/main/res/values/themes.xml`
```xml
<resources>
    <style name="Theme.ExpenseTracker" parent="Theme.Material3.DayNight.NoActionBar">
        <item name="colorPrimary">@color/primary</item>
        <item name="colorOnPrimary">@color/on_primary</item>
        <item name="colorPrimaryContainer">@color/primary_container</item>
        <item name="colorOnPrimaryContainer">@color/primary</item>
        <item name="colorSecondary">@color/secondary</item>
        <item name="colorSecondaryContainer">@color/primary_container</item>
        <item name="colorOnSecondaryContainer">@color/primary</item>
        <item name="colorSurface">@color/card</item>
        <item name="colorSurfaceContainer">@color/card</item>
        <item name="colorOnSurface">@color/text_primary</item>
        <item name="colorOnSurfaceVariant">@color/text_secondary</item>
        <item name="android:colorBackground">@color/bg</item>
        <item name="android:statusBarColor">@color/primary_dark</item>
        <item name="android:navigationBarColor">@color/card</item>
    </style>

    <!-- Text styles (no dots in names so no parent style is required) -->
    <style name="TextHeading">
        <item name="android:textSize">24sp</item>
        <item name="android:textStyle">bold</item>
        <item name="android:textColor">@color/text_primary</item>
    </style>
    <style name="TextTitle">
        <item name="android:textSize">17sp</item>
        <item name="android:textStyle">bold</item>
        <item name="android:textColor">@color/text_primary</item>
    </style>
    <style name="TextBody">
        <item name="android:textSize">14sp</item>
        <item name="android:textColor">@color/text_primary</item>
    </style>
    <style name="TextCaption">
        <item name="android:textSize">12sp</item>
        <item name="android:textColor">@color/text_secondary</item>
    </style>

    <!-- Rounded card -->
    <style name="AppCard" parent="Widget.Material3.CardView.Filled">
        <item name="cardCornerRadius">20dp</item>
        <item name="cardBackgroundColor">@color/card</item>
        <item name="cardElevation">0dp</item>
    </style>

    <!-- Rounded buttons -->
    <style name="AppButton" parent="Widget.Material3.Button">
        <item name="cornerRadius">16dp</item>
    </style>

    <!-- Rounded text fields -->
    <style name="ShapeInput">
        <item name="cornerFamily">rounded</item>
        <item name="cornerSize">14dp</item>
    </style>
    <style name="AppInput" parent="Widget.Material3.TextInputLayout.OutlinedBox">
        <item name="shapeAppearance">@style/ShapeInput</item>
    </style>
    <style name="AppDropdown" parent="Widget.Material3.TextInputLayout.OutlinedBox.ExposedDropdownMenu">
        <item name="shapeAppearance">@style/ShapeInput</item>
    </style>
</resources>
```

### `app/src/main/res/menu/bottom_nav.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/nav_home" android:icon="@drawable/ic_home" android:title="Home" />
    <item android:id="@+id/nav_transactions" android:icon="@drawable/ic_list" android:title="Transactions" />
    <item android:id="@+id/nav_stats" android:icon="@drawable/ic_chart" android:title="Statistics" />
    <item android:id="@+id/nav_settings" android:icon="@drawable/ic_settings" android:title="Settings" />
</menu>
```

### Drawables (`app/src/main/res/drawable/`)

`bg_gradient_card.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
    <gradient android:startColor="@color/gradient_start" android:endColor="@color/gradient_end" android:angle="315" />
    <corners android:radius="24dp" />
</shape>
```

`bg_splash.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
    <gradient android:startColor="@color/gradient_start" android:endColor="@color/gradient_end" android:angle="270" />
</shape>
```

`bg_circle.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="oval">
    <solid android:color="@color/white" />
</shape>
```

`ic_home.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:width="24dp" android:height="24dp" android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000" android:pathData="M10,20v-6h4v6h5v-8h3L12,3 2,12h3v8z" />
</vector>
```

`ic_list.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:width="24dp" android:height="24dp" android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000" android:pathData="M3,13h2v-2L3,11v2zM3,17h2v-2L3,15v2zM3,9h2L5,7L3,7v2zM7,13h14v-2L7,11v2zM7,17h14v-2L7,15v2zM7,7v2h14L21,7L7,7z" />
</vector>
```

`ic_chart.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:width="24dp" android:height="24dp" android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000" android:pathData="M5,9.2h3L8,19L5,19zM10.6,5h2.8v14h-2.8zM16.2,13L19,13v6h-2.8z" />
</vector>
```

`ic_settings.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:width="24dp" android:height="24dp" android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000" android:pathData="M19.14,12.94c0.04,-0.3 0.06,-0.61 0.06,-0.94c0,-0.32 -0.02,-0.64 -0.07,-0.94l2.03,-1.58c0.18,-0.14 0.23,-0.41 0.12,-0.61l-1.92,-3.32c-0.12,-0.22 -0.37,-0.29 -0.59,-0.22l-2.39,0.96c-0.5,-0.38 -1.03,-0.7 -1.62,-0.94L14.4,2.81c-0.04,-0.24 -0.24,-0.41 -0.48,-0.41h-3.84c-0.24,0 -0.43,0.17 -0.47,0.41L9.25,5.35C8.66,5.59 8.12,5.92 7.63,6.29L5.24,5.33c-0.22,-0.08 -0.47,0 -0.59,0.22L2.74,8.87C2.62,9.08 2.66,9.34 2.86,9.48l2.03,1.58C4.84,11.36 4.8,11.69 4.8,12s0.02,0.64 0.07,0.94l-2.03,1.58c-0.18,0.14 -0.23,0.41 -0.12,0.61l1.92,3.32c0.12,0.22 0.37,0.29 0.59,0.22l2.39,-0.96c0.5,0.38 1.03,0.7 1.62,0.94l0.36,2.54c0.05,0.24 0.24,0.41 0.48,0.41h3.84c0.24,0 0.44,-0.17 0.47,-0.41l0.36,-2.54c0.59,-0.24 1.13,-0.56 1.62,-0.94l2.39,0.96c0.22,0.08 0.47,0 0.59,-0.22l1.92,-3.32c0.12,-0.22 0.07,-0.47 -0.12,-0.61L19.14,12.94zM12,15.6c-1.98,0 -3.6,-1.62 -3.6,-3.6s1.62,-3.6 3.6,-3.6s3.6,1.62 3.6,3.6S13.98,15.6 12,15.6z" />
</vector>
```

`ic_edit.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:width="24dp" android:height="24dp" android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000" android:pathData="M3,17.25V21h3.75L17.81,9.94l-3.75,-3.75L3,17.25zM20.71,7.04c0.39,-0.39 0.39,-1.02 0,-1.41l-2.34,-2.34c-0.39,-0.39 -1.02,-0.39 -1.41,0l-1.83,1.83 3.75,3.75 1.83,-1.83z" />
</vector>
```

`ic_delete.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:width="24dp" android:height="24dp" android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000" android:pathData="M6,19c0,1.1 0.9,2 2,2h8c1.1,0 2,-0.9 2,-2V7H6v12zM19,4h-3.5l-1,-1h-5l-1,1H5v2h14V4z" />
</vector>
```

`ic_back.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:width="24dp" android:height="24dp" android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000" android:pathData="M20,11H7.83l5.59,-5.59L12,4l-8,8 8,8 1.41,-1.41L7.83,13H20v-2z" />
</vector>
```

`ic_search.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:width="24dp" android:height="24dp" android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000" android:pathData="M15.5,14h-0.79l-0.28,-0.27C15.41,12.59 16,11.11 16,9.5 16,5.91 13.09,3 9.5,3S3,5.91 3,9.5 5.91,16 9.5,16c1.61,0 3.09,-0.59 4.23,-1.57l0.27,0.28v0.79l5,4.99L20.49,19l-4.99,-5zM9.5,14C7.01,14 5,11.99 5,9.5S7.01,5 9.5,5 14,7.01 14,9.5 11.99,14 9.5,14z" />
</vector>
```

`ic_wallet.xml` (app logo)
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:width="24dp" android:height="24dp" android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF000000" android:pathData="M21,18v1c0,1.1 -0.9,2 -2,2H5c-1.11,0 -2,-0.9 -2,-2V5c0,-1.1 0.89,-2 2,-2h14c1.1,0 2,0.9 2,2v1h-9c-1.11,0 -2,0.9 -2,2v8c0,1.1 0.89,2 2,2h9zM12,16h10V8H12v8zM16,13.5c-0.83,0 -1.5,-0.67 -1.5,-1.5s0.67,-1.5 1.5,-1.5 1.5,0.67 1.5,1.5 -0.67,1.5 -1.5,1.5z" />
</vector>
```

---

# PART 4 – KOTLIN (package `com.example.expense`)

### `ExpenseApp.kt`
```kotlin
package com.example.expense

import android.app.Application
import androidx.appcompat.app.AppCompatDelegate
import com.example.expense.data.Utils
import com.example.expense.database.AppDatabase
import kotlinx.coroutines.runBlocking

/** Runs once when the app starts: applies the saved theme and currency. */
class ExpenseApp : Application() {
    override fun onCreate() {
        super.onCreate()
        val dao = AppDatabase.get(this).prefDao()
        // runBlocking is OK here: it is a tiny read and the theme must be set before any screen shows.
        val (theme, currency) = runBlocking {
            Pair(dao.get("theme")?.prefValue ?: "system", dao.get("currency")?.prefValue ?: "₹")
        }
        Utils.currency = currency
        AppCompatDelegate.setDefaultNightMode(Utils.nightMode(theme))
    }
}
```

### `data/Constants.kt`
```kotlin
package com.example.expense.data

object Constants {
    const val INCOME = "INCOME"
    const val EXPENSE = "EXPENSE"

    val EXPENSE_CATEGORIES = listOf(
        "Food", "Shopping", "Travel", "Bills", "Education", "Health", "Entertainment", "Other"
    )
    val INCOME_CATEGORIES = listOf("Salary", "Business", "Gift", "Investment", "Other")
    val PAYMENT_METHODS = listOf("Cash", "Card", "UPI", "Net Banking", "Other")
}
```

### `data/Entities.kt`
```kotlin
package com.example.expense.data

import androidx.room.Entity
import androidx.room.PrimaryKey

/** One income or expense record. date = midnight (local time) in milliseconds. */
@Entity(tableName = "transactions")
data class ExpenseTransaction(
    @PrimaryKey(autoGenerate = true) val id: Long = 0,
    val type: String,            // Constants.INCOME or Constants.EXPENSE
    val amount: Double,
    val category: String,
    val date: Long,
    val paymentMethod: String,
    val description: String
)

@Entity(tableName = "categories")
data class Category(
    @PrimaryKey(autoGenerate = true) val id: Long = 0,
    val name: String,
    val isExpense: Boolean
)

/** One budget per month. month looks like "2026-11". */
@Entity(tableName = "budgets")
data class Budget(
    @PrimaryKey val month: String,
    val amount: Double
)

/** Simple key/value settings: "name", "theme", "currency". */
@Entity(tableName = "preferences")
data class UserPref(
    @PrimaryKey val prefKey: String,
    val prefValue: String
)
```

### `data/Utils.kt`
```kotlin
package com.example.expense.data

import android.graphics.Color
import androidx.appcompat.app.AppCompatDelegate
import java.text.SimpleDateFormat
import java.util.Calendar
import java.util.Date
import java.util.Locale
import kotlin.math.abs

object Utils {
    /** Current currency symbol (loaded from Room at app start). */
    var currency = "₹"

    fun money(amount: Double): String =
        currency + String.format(Locale.getDefault(), "%,.2f", amount)

    fun formatDate(ms: Long): String =
        SimpleDateFormat("dd MMM yyyy", Locale.getDefault()).format(Date(ms))

    fun monthKey(): String =
        SimpleDateFormat("yyyy-MM", Locale.US).format(Date())

    fun nightMode(mode: String): Int = when (mode) {
        "light" -> AppCompatDelegate.MODE_NIGHT_NO
        "dark" -> AppCompatDelegate.MODE_NIGHT_YES
        else -> AppCompatDelegate.MODE_NIGHT_FOLLOW_SYSTEM
    }

    // A calendar set to 00:00:00.000 of the given day
    private fun cal(ms: Long = System.currentTimeMillis()): Calendar =
        Calendar.getInstance().apply {
            timeInMillis = ms
            set(Calendar.HOUR_OF_DAY, 0)
            set(Calendar.MINUTE, 0)
            set(Calendar.SECOND, 0)
            set(Calendar.MILLISECOND, 0)
        }

    fun startOfDay(ms: Long): Long = cal(ms).timeInMillis

    fun endOfDay(ms: Long): Long = cal(ms).apply {
        add(Calendar.DAY_OF_MONTH, 1)
        add(Calendar.MILLISECOND, -1)
    }.timeInMillis

    /** Midnight of the day "daysAgo" days before today. */
    fun dayStartAgo(daysAgo: Int): Long = cal().apply { add(Calendar.DAY_OF_MONTH, -daysAgo) }.timeInMillis

    /** Date range for a period: 0 = today, 1 = this week (Mon-Sun), 2 = this month. */
    fun periodRange(period: Int): Pair<Long, Long> {
        val c = cal()
        return when (period) {
            0 -> Pair(c.timeInMillis, endOfDay(c.timeInMillis))
            1 -> {
                val diff = (c.get(Calendar.DAY_OF_WEEK) - Calendar.MONDAY + 7) % 7
                c.add(Calendar.DAY_OF_MONTH, -diff)
                val start = c.timeInMillis
                c.add(Calendar.DAY_OF_MONTH, 6)
                Pair(start, endOfDay(c.timeInMillis))
            }
            else -> {
                c.set(Calendar.DAY_OF_MONTH, 1)
                val start = c.timeInMillis
                c.set(Calendar.DAY_OF_MONTH, c.getActualMaximum(Calendar.DAY_OF_MONTH))
                Pair(start, endOfDay(c.timeInMillis))
            }
        }
    }

    private val palette = listOf(
        "#5B4BDB", "#3F8CFF", "#1FA971", "#F5A524", "#E5484D", "#12A594", "#D6409F", "#8E8E93"
    ).map { Color.parseColor(it) }

    /** Same category always gets the same color (used in list icons and charts). */
    fun categoryColor(name: String): Int {
        val i = Constants.EXPENSE_CATEGORIES.indexOf(name)
        return palette[if (i >= 0) i else abs(name.hashCode()) % palette.size]
    }
}
```

### `database/Daos.kt`
```kotlin
package com.example.expense.database

import androidx.lifecycle.LiveData
import androidx.room.Dao
import androidx.room.Delete
import androidx.room.Insert
import androidx.room.OnConflictStrategy
import androidx.room.Query
import androidx.room.Update
import com.example.expense.data.Budget
import com.example.expense.data.Category
import com.example.expense.data.ExpenseTransaction
import com.example.expense.data.UserPref

@Dao
interface TransactionDao {
    @Insert suspend fun insert(t: ExpenseTransaction)
    @Insert suspend fun insertAll(list: List<ExpenseTransaction>)
    @Update suspend fun update(t: ExpenseTransaction)
    @Delete suspend fun delete(t: ExpenseTransaction)

    @Query("DELETE FROM transactions") suspend fun deleteAll()

    @Query("SELECT * FROM transactions WHERE id = :id")
    suspend fun getById(id: Long): ExpenseTransaction?

    // LiveData queries update the screen automatically whenever the table changes
    @Query("SELECT * FROM transactions ORDER BY date DESC, id DESC")
    fun getAll(): LiveData<List<ExpenseTransaction>>

    @Query("SELECT * FROM transactions WHERE date BETWEEN :start AND :end ORDER BY date DESC, id DESC")
    fun getBetween(start: Long, end: Long): LiveData<List<ExpenseTransaction>>

    @Query("SELECT SUM(amount) FROM transactions WHERE type = :type")
    fun totalByType(type: String): LiveData<Double?>

    @Query("SELECT SUM(amount) FROM transactions WHERE type = 'EXPENSE' AND date BETWEEN :start AND :end")
    fun expenseBetween(start: Long, end: Long): LiveData<Double?>

    @Query("SELECT SUM(amount) FROM transactions WHERE type = 'EXPENSE' AND date BETWEEN :start AND :end")
    suspend fun expenseSumNow(start: Long, end: Long): Double?
}

@Dao
interface CategoryDao {
    @Insert suspend fun insertAll(list: List<Category>)

    @Query("SELECT name FROM categories WHERE isExpense = :isExpense ORDER BY id")
    suspend fun getNames(isExpense: Boolean): List<String>

    @Query("SELECT DISTINCT name FROM categories ORDER BY name")
    fun getAllNames(): LiveData<List<String>>
}

@Dao
interface BudgetDao {
    @Insert(onConflict = OnConflictStrategy.REPLACE) suspend fun set(b: Budget)

    @Query("SELECT * FROM budgets WHERE month = :month")
    fun get(month: String): LiveData<Budget?>

    @Query("SELECT * FROM budgets WHERE month = :month")
    suspend fun getNow(month: String): Budget?

    @Query("DELETE FROM budgets") suspend fun deleteAll()
}

@Dao
interface PrefDao {
    @Insert(onConflict = OnConflictStrategy.REPLACE) suspend fun set(p: UserPref)

    @Query("SELECT * FROM preferences WHERE prefKey = :key")
    suspend fun get(key: String): UserPref?

    @Query("SELECT * FROM preferences WHERE prefKey = :key")
    fun observe(key: String): LiveData<UserPref?>
}
```

### `database/AppDatabase.kt`
```kotlin
package com.example.expense.database

import android.content.Context
import androidx.room.Database
import androidx.room.Room
import androidx.room.RoomDatabase
import androidx.sqlite.db.SupportSQLiteDatabase
import com.example.expense.data.Budget
import com.example.expense.data.Category
import com.example.expense.data.Constants
import com.example.expense.data.ExpenseTransaction
import com.example.expense.data.UserPref

@Database(
    entities = [ExpenseTransaction::class, Category::class, Budget::class, UserPref::class],
    version = 1,
    exportSchema = false
)
abstract class AppDatabase : RoomDatabase() {

    abstract fun transactionDao(): TransactionDao
    abstract fun categoryDao(): CategoryDao
    abstract fun budgetDao(): BudgetDao
    abstract fun prefDao(): PrefDao

    companion object {
        @Volatile private var INSTANCE: AppDatabase? = null

        // Singleton: only one database object exists in the whole app
        fun get(context: Context): AppDatabase = INSTANCE ?: synchronized(this) {
            INSTANCE ?: Room.databaseBuilder(
                context.applicationContext, AppDatabase::class.java, "expense_db"
            ).addCallback(object : Callback() {
                // Runs only the first time the database file is created: add default categories
                override fun onCreate(db: SupportSQLiteDatabase) {
                    super.onCreate(db)
                    Constants.EXPENSE_CATEGORIES.forEach {
                        db.execSQL("INSERT INTO categories (name, isExpense) VALUES ('$it', 1)")
                    }
                    Constants.INCOME_CATEGORIES.forEach {
                        db.execSQL("INSERT INTO categories (name, isExpense) VALUES ('$it', 0)")
                    }
                }
            }).build().also { INSTANCE = it }
        }
    }
}
```

### `repository/AppRepository.kt`
```kotlin
package com.example.expense.repository

import com.example.expense.database.AppDatabase

/** Single place that gives the ViewModel access to all data sources. */
class AppRepository(db: AppDatabase) {
    val transactions = db.transactionDao()
    val categories = db.categoryDao()
    val budgets = db.budgetDao()
    val prefs = db.prefDao()
}
```

### `viewmodel/MainViewModel.kt`
```kotlin
package com.example.expense.viewmodel

import android.app.Application
import androidx.lifecycle.AndroidViewModel
import androidx.lifecycle.LiveData
import androidx.lifecycle.MediatorLiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.map
import androidx.lifecycle.switchMap
import androidx.lifecycle.viewModelScope
import com.example.expense.data.Budget
import com.example.expense.data.Constants
import com.example.expense.data.ExpenseTransaction
import com.example.expense.data.UserPref
import com.example.expense.data.Utils
import com.example.expense.database.AppDatabase
import com.example.expense.repository.AppRepository
import kotlinx.coroutines.launch

/** One ViewModel shared by all screens. It exposes LiveData and functions to change data. */
class MainViewModel(app: Application) : AndroidViewModel(app) {

    private val repo = AppRepository(AppDatabase.get(app))
    private val txDao = repo.transactions

    // ---------- Dashboard ----------
    val allTransactions: LiveData<List<ExpenseTransaction>> = txDao.getAll()
    val totalIncome: LiveData<Double> = txDao.totalByType(Constants.INCOME).map { it ?: 0.0 }
    val totalExpense: LiveData<Double> = txDao.totalByType(Constants.EXPENSE).map { it ?: 0.0 }

    // balance = income - expense (recalculated whenever either changes)
    val balance = MediatorLiveData<Double>().apply {
        val update = { value = (totalIncome.value ?: 0.0) - (totalExpense.value ?: 0.0) }
        addSource(totalIncome) { update() }
        addSource(totalExpense) { update() }
    }

    val monthBudget: LiveData<Double> =
        repo.budgets.get(Utils.monthKey()).map { it?.amount ?: 0.0 }

    val monthExpense: LiveData<Double> = run {
        val (start, end) = Utils.periodRange(2)
        txDao.expenseBetween(start, end).map { it ?: 0.0 }
    }

    // ---------- Search & filter ----------
    val query = MutableLiveData("")
    val typeFilter = MutableLiveData("ALL")      // "ALL", INCOME or EXPENSE
    val categoryFilter = MutableLiveData("All")
    val allCategoryNames: LiveData<List<String>> = repo.categories.getAllNames()

    val filtered = MediatorLiveData<List<ExpenseTransaction>>().apply {
        val update = {
            val q = query.value.orEmpty().trim().lowercase()
            val type = typeFilter.value
            val cat = categoryFilter.value
            value = (allTransactions.value ?: emptyList()).filter { t ->
                (type == "ALL" || t.type == type) &&
                (cat == "All" || t.category == cat) &&
                (q.isEmpty() ||
                    t.description.lowercase().contains(q) ||
                    t.category.lowercase().contains(q) ||
                    t.paymentMethod.lowercase().contains(q) ||
                    t.amount.toString().contains(q))
            }
        }
        addSource(allTransactions) { update() }
        addSource(query) { update() }
        addSource(typeFilter) { update() }
        addSource(categoryFilter) { update() }
    }

    // ---------- Statistics ----------
    val period = MutableLiveData(0)   // 0 daily, 1 weekly, 2 monthly
    val periodTransactions: LiveData<List<ExpenseTransaction>> = period.switchMap { p ->
        val (start, end) = Utils.periodRange(p)
        txDao.getBetween(start, end)
    }
    val last7Days: LiveData<List<ExpenseTransaction>> =
        txDao.getBetween(Utils.dayStartAgo(6), Utils.endOfDay(System.currentTimeMillis()))

    // ---------- Settings ----------
    val userName: LiveData<String> = repo.prefs.observe("name").map { it?.prefValue ?: "User" }
    val themeMode: LiveData<String> = repo.prefs.observe("theme").map { it?.prefValue ?: "system" }

    fun savePref(key: String, value: String) = viewModelScope.launch {
        repo.prefs.set(UserPref(key, value))
    }

    fun setBudget(amount: Double) = viewModelScope.launch {
        repo.budgets.set(Budget(Utils.monthKey(), amount))
    }

    // ---------- Transactions ----------
    suspend fun getTransaction(id: Long) = txDao.getById(id)
    suspend fun categoryNames(isExpense: Boolean) = repo.categories.getNames(isExpense)

    fun delete(t: ExpenseTransaction) = viewModelScope.launch { txDao.delete(t) }

    /** Saves the transaction. For expenses, returns a warning text if the monthly budget is near/over. */
    suspend fun saveAndCheck(t: ExpenseTransaction, isEdit: Boolean): String? {
        if (isEdit) txDao.update(t) else txDao.insert(t)
        if (t.type != Constants.EXPENSE) return null
        val budget = repo.budgets.getNow(Utils.monthKey())?.amount ?: return null
        if (budget <= 0) return null
        val (start, end) = Utils.periodRange(2)
        val spent = txDao.expenseSumNow(start, end) ?: 0.0
        return when {
            spent >= budget -> "⚠ Monthly budget exceeded!"
            spent >= budget * 0.8 -> "⚠ You have used ${(spent / budget * 100).toInt()}% of your monthly budget"
            else -> null
        }
    }

    // ---------- Sample data / reset ----------
    private fun tx(type: String, amount: Double, cat: String, daysAgo: Int, pay: String, desc: String) =
        ExpenseTransaction(
            type = type, amount = amount, category = cat, date = Utils.dayStartAgo(daysAgo),
            paymentMethod = pay, description = desc
        )

    fun loadSampleData() = viewModelScope.launch {
        val inc = Constants.INCOME
        val exp = Constants.EXPENSE
        txDao.insertAll(
            listOf(
                tx(inc, 45000.0, "Salary", 25, "Net Banking", "Monthly salary"),
                tx(inc, 5000.0, "Business", 12, "UPI", "Freelance project"),
                tx(inc, 1500.0, "Gift", 8, "Cash", "Birthday gift"),
                tx(exp, 250.0, "Food", 0, "UPI", "Lunch at canteen"),
                tx(exp, 480.0, "Food", 1, "Card", "Dinner with friends"),
                tx(exp, 120.0, "Food", 2, "Cash", "Tea and snacks"),
                tx(exp, 320.0, "Food", 4, "UPI", "Pizza"),
                tx(exp, 1500.0, "Food", 6, "Card", "Groceries"),
                tx(exp, 150.0, "Travel", 1, "Cash", "Auto fare"),
                tx(exp, 300.0, "Travel", 3, "UPI", "Cab ride"),
                tx(exp, 1200.0, "Travel", 9, "UPI", "Train ticket"),
                tx(exp, 2499.0, "Shopping", 3, "Card", "New headphones"),
                tx(exp, 899.0, "Shopping", 14, "UPI", "T-shirt"),
                tx(exp, 1100.0, "Bills", 5, "Net Banking", "Electricity bill"),
                tx(exp, 399.0, "Bills", 6, "UPI", "Mobile recharge"),
                tx(exp, 1800.0, "Education", 10, "Card", "Online course"),
                tx(exp, 350.0, "Education", 4, "Cash", "Notebooks and pens"),
                tx(exp, 600.0, "Health", 7, "Cash", "Medicines"),
                tx(exp, 450.0, "Entertainment", 2, "UPI", "Movie tickets"),
                tx(exp, 199.0, "Entertainment", 11, "Card", "Streaming subscription"),
                tx(exp, 500.0, "Other", 13, "Cash", "Donation")
            )
        )
        repo.budgets.set(Budget(Utils.monthKey(), 20000.0))
    }

    fun clearAll() = viewModelScope.launch {
        txDao.deleteAll()
        repo.budgets.deleteAll()
    }
}
```

### `ui/SplashActivity.kt`
```kotlin
package com.example.expense.ui

import android.content.Intent
import android.os.Bundle
import android.os.Handler
import android.os.Looper
import androidx.appcompat.app.AppCompatActivity
import com.example.expense.R

class SplashActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_splash)
        // Show the logo for 1.5 seconds, then open the main screen
        Handler(Looper.getMainLooper()).postDelayed({
            startActivity(Intent(this, MainActivity::class.java))
            finish()
        }, 1500)
    }
}
```

### `ui/MainActivity.kt`
```kotlin
package com.example.expense.ui

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import com.example.expense.R
import com.example.expense.databinding.ActivityMainBinding

/** Hosts the 4 tabs. Bottom navigation swaps the fragment shown in the container. */
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.bottomNav.setOnItemSelectedListener { item ->
            val fragment: Fragment = when (item.itemId) {
                R.id.nav_home -> HomeFragment()
                R.id.nav_transactions -> TransactionsFragment()
                R.id.nav_stats -> StatsFragment()
                else -> SettingsFragment()
            }
            supportFragmentManager.beginTransaction()
                .replace(R.id.container, fragment)
                .commit()
            true
        }

        // First launch: show Home (after rotation/theme change Android restores the old tab)
        if (savedInstanceState == null) binding.bottomNav.selectedItemId = R.id.nav_home
    }

    /** Lets a fragment switch tabs (used by "See all" on Home). */
    fun selectTab(id: Int) {
        binding.bottomNav.selectedItemId = id
    }
}
```

### `ui/AddTransactionActivity.kt`
```kotlin
package com.example.expense.ui

import android.app.DatePickerDialog
import android.content.Context
import android.content.Intent
import android.os.Bundle
import android.widget.ArrayAdapter
import android.widget.Toast
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import com.example.expense.data.Constants
import com.example.expense.data.ExpenseTransaction
import com.example.expense.data.Utils
import com.example.expense.databinding.ActivityAddTransactionBinding
import com.example.expense.viewmodel.MainViewModel
import kotlinx.coroutines.launch
import java.util.Calendar

/** Used for BOTH adding and editing, and for both income and expense. */
class AddTransactionActivity : AppCompatActivity() {

    private lateinit var b: ActivityAddTransactionBinding
    private val vm: MainViewModel by viewModels()

    private var type = Constants.EXPENSE
    private var editId = -1L                       // -1 means "adding a new one"
    private var selectedDate = Utils.startOfDay(System.currentTimeMillis())

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        b = ActivityAddTransactionBinding.inflate(layoutInflater)
        setContentView(b.root)

        type = intent.getStringExtra(EXTRA_TYPE) ?: Constants.EXPENSE
        editId = intent.getLongExtra(EXTRA_ID, -1L)

        b.toolbar.setNavigationOnClickListener { finish() }
        b.tilAmount.prefixText = Utils.currency
        b.etDate.setOnClickListener { showDatePicker() }
        b.actPayment.setAdapter(
            ArrayAdapter(this, android.R.layout.simple_list_item_1, Constants.PAYMENT_METHODS)
        )
        b.actPayment.setText(Constants.PAYMENT_METHODS[0], false)
        updateDateText()

        if (editId == -1L) setupCategories(null) else loadForEdit()
        b.btnSave.setOnClickListener { save() }
    }

    /** Fills the category dropdown (expense or income list) and the screen title. */
    private fun setupCategories(selected: String?) {
        val word = if (type == Constants.EXPENSE) "Expense" else "Income"
        b.toolbar.title = (if (editId == -1L) "Add " else "Edit ") + word
        lifecycleScope.launch {
            val names = vm.categoryNames(type == Constants.EXPENSE)
            b.actCategory.setAdapter(ArrayAdapter(this@AddTransactionActivity, android.R.layout.simple_list_item_1, names))
            if (selected != null) b.actCategory.setText(selected, false)
        }
    }

    private fun loadForEdit() {
        lifecycleScope.launch {
            val t = vm.getTransaction(editId)
            if (t == null) { finish(); return@launch }
            type = t.type
            selectedDate = t.date
            b.etAmount.setText(t.amount.toString())
            b.actPayment.setText(t.paymentMethod, false)
            b.etDescription.setText(t.description)
            updateDateText()
            setupCategories(t.category)
        }
    }

    private fun showDatePicker() {
        val c = Calendar.getInstance().apply { timeInMillis = selectedDate }
        val dialog = DatePickerDialog(this, { _, y, m, d ->
            selectedDate = Calendar.getInstance().apply {
                set(y, m, d, 0, 0, 0)
                set(Calendar.MILLISECOND, 0)
            }.timeInMillis
            updateDateText()
        }, c.get(Calendar.YEAR), c.get(Calendar.MONTH), c.get(Calendar.DAY_OF_MONTH))
        dialog.datePicker.maxDate = System.currentTimeMillis()   // no future dates
        dialog.show()
    }

    private fun updateDateText() {
        b.etDate.setText(Utils.formatDate(selectedDate))
    }

    private fun save() {
        // ---- validation ----
        val amount = b.etAmount.text.toString().trim().toDoubleOrNull()
        val category = b.actCategory.text.toString().trim()
        val payment = b.actPayment.text.toString().trim()

        b.tilAmount.error = when {
            amount == null || amount <= 0 -> "Enter an amount greater than 0"
            amount > 1_000_000_000 -> "Amount is too large"
            else -> null
        }
        b.tilCategory.error = if (category.isEmpty()) "Select a category" else null
        b.tilPayment.error = if (payment.isEmpty()) "Select a payment method" else null
        if (b.tilAmount.error != null || b.tilCategory.error != null || b.tilPayment.error != null) return

        val transaction = ExpenseTransaction(
            id = if (editId == -1L) 0 else editId,
            type = type,
            amount = amount!!,
            category = category,
            date = selectedDate,
            paymentMethod = payment,
            description = b.etDescription.text.toString().trim()
        )

        b.btnSave.isEnabled = false      // prevent double taps
        lifecycleScope.launch {
            val warning = vm.saveAndCheck(transaction, editId != -1L)
            Toast.makeText(this@AddTransactionActivity, "Saved", Toast.LENGTH_SHORT).show()
            if (warning != null) Toast.makeText(this@AddTransactionActivity, warning, Toast.LENGTH_LONG).show()
            finish()
        }
    }

    companion object {
        const val EXTRA_TYPE = "type"
        const val EXTRA_ID = "id"

        fun intent(context: Context, type: String, id: Long = -1L): Intent =
            Intent(context, AddTransactionActivity::class.java)
                .putExtra(EXTRA_TYPE, type)
                .putExtra(EXTRA_ID, id)
    }
}
```

### `ui/TransactionAdapter.kt`
```kotlin
package com.example.expense.ui

import android.content.res.ColorStateList
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.core.content.ContextCompat
import androidx.recyclerview.widget.DiffUtil
import androidx.recyclerview.widget.ListAdapter
import androidx.recyclerview.widget.RecyclerView
import com.example.expense.R
import com.example.expense.data.Constants
import com.example.expense.data.ExpenseTransaction
import com.example.expense.data.Utils
import com.example.expense.databinding.ItemTransactionBinding

/** Shows a list of transactions. ListAdapter + DiffUtil animates changes efficiently. */
class TransactionAdapter(
    private val onEdit: (ExpenseTransaction) -> Unit,
    private val onDelete: (ExpenseTransaction) -> Unit,
    private val showActions: Boolean = true
) : ListAdapter<ExpenseTransaction, TransactionAdapter.VH>(Diff) {

    class VH(val b: ItemTransactionBinding) : RecyclerView.ViewHolder(b.root)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int) =
        VH(ItemTransactionBinding.inflate(LayoutInflater.from(parent.context), parent, false))

    override fun onBindViewHolder(holder: VH, position: Int) {
        val t = getItem(position)
        val ctx = holder.itemView.context
        with(holder.b) {
            tvCategory.text = t.category
            tvInitial.text = t.category.take(1).uppercase()
            tvInitial.backgroundTintList = ColorStateList.valueOf(Utils.categoryColor(t.category))

            tvInfo.text = buildString {
                append(Utils.formatDate(t.date))
                append(" • ").append(t.paymentMethod)
                if (t.description.isNotBlank()) append(" • ").append(t.description)
            }

            val isIncome = t.type == Constants.INCOME
            tvAmount.text = (if (isIncome) "+" else "-") + Utils.money(t.amount)
            tvAmount.setTextColor(ContextCompat.getColor(ctx, if (isIncome) R.color.income else R.color.expense))

            val vis = if (showActions) View.VISIBLE else View.GONE
            btnEdit.visibility = vis
            btnDelete.visibility = vis
            btnEdit.setOnClickListener { onEdit(t) }
            btnDelete.setOnClickListener { onDelete(t) }
            root.setOnClickListener { onEdit(t) }
        }
    }

    object Diff : DiffUtil.ItemCallback<ExpenseTransaction>() {
        override fun areItemsTheSame(a: ExpenseTransaction, b: ExpenseTransaction) = a.id == b.id
        override fun areContentsTheSame(a: ExpenseTransaction, b: ExpenseTransaction) = a == b
    }
}
```

### `ui/HomeFragment.kt`
```kotlin
package com.example.expense.ui

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.core.content.ContextCompat
import androidx.fragment.app.Fragment
import androidx.fragment.app.activityViewModels
import androidx.recyclerview.widget.LinearLayoutManager
import com.example.expense.R
import com.example.expense.data.Constants
import com.example.expense.data.Utils
import com.example.expense.databinding.FragmentHomeBinding
import com.example.expense.viewmodel.MainViewModel

class HomeFragment : Fragment() {

    private var _b: FragmentHomeBinding? = null
    private val b get() = _b!!
    private val vm: MainViewModel by activityViewModels()

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View {
        _b = FragmentHomeBinding.inflate(inflater, container, false)
        return b.root
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        val adapter = TransactionAdapter(
            onEdit = { startActivity(AddTransactionActivity.intent(requireContext(), it.type, it.id)) },
            onDelete = {},
            showActions = false
        )
        b.rvRecent.layoutManager = LinearLayoutManager(requireContext())
        b.rvRecent.adapter = adapter

        vm.userName.observe(viewLifecycleOwner) { b.tvGreeting.text = "Hello, $it" }
        vm.balance.observe(viewLifecycleOwner) { b.tvBalance.text = Utils.money(it) }
        vm.totalIncome.observe(viewLifecycleOwner) { b.tvIncome.text = Utils.money(it) }
        vm.totalExpense.observe(viewLifecycleOwner) { b.tvExpense.text = Utils.money(it) }
        vm.allTransactions.observe(viewLifecycleOwner) { list ->
            adapter.submitList(list.take(5))                       // latest 5
            b.tvEmpty.visibility = if (list.isEmpty()) View.VISIBLE else View.GONE
        }
        vm.monthBudget.observe(viewLifecycleOwner) { updateBudget() }
        vm.monthExpense.observe(viewLifecycleOwner) { updateBudget() }

        b.btnAddExpense.setOnClickListener {
            startActivity(AddTransactionActivity.intent(requireContext(), Constants.EXPENSE))
        }
        b.btnAddIncome.setOnClickListener {
            startActivity(AddTransactionActivity.intent(requireContext(), Constants.INCOME))
        }
        b.tvSeeAll.setOnClickListener {
            (requireActivity() as MainActivity).selectTab(R.id.nav_transactions)
        }
    }

    /** Budget card: progress bar + warning at 80% and 100%. */
    private fun updateBudget() {
        val budget = vm.monthBudget.value ?: 0.0
        val spent = vm.monthExpense.value ?: 0.0
        if (budget <= 0) {
            b.tvBudgetInfo.text = "No budget set. Go to Settings to set one."
            b.progressBudget.setProgressCompat(0, false)
            b.tvBudgetWarning.visibility = View.GONE
            return
        }
        val pct = (spent / budget * 100).toInt()
        b.tvBudgetInfo.text = "${Utils.money(spent)} of ${Utils.money(budget)} spent ($pct%)"
        b.progressBudget.setProgressCompat(pct.coerceAtMost(100), true)

        val colorRes = when {
            pct >= 100 -> R.color.expense
            pct >= 80 -> R.color.warning
            else -> R.color.primary
        }
        val color = ContextCompat.getColor(requireContext(), colorRes)
        b.progressBudget.setIndicatorColor(color)

        when {
            pct >= 100 -> showWarning("⚠ Budget exceeded by ${Utils.money(spent - budget)}!", color)
            pct >= 80 -> showWarning("⚠ Warning: you have used $pct% of your monthly budget", color)
            else -> b.tvBudgetWarning.visibility = View.GONE
        }
    }

    private fun showWarning(text: String, color: Int) {
        b.tvBudgetWarning.text = text
        b.tvBudgetWarning.setTextColor(color)
        b.tvBudgetWarning.visibility = View.VISIBLE
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _b = null
    }
}
```

### `ui/TransactionsFragment.kt`
```kotlin
package com.example.expense.ui

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ArrayAdapter
import android.widget.Toast
import androidx.core.widget.doAfterTextChanged
import androidx.fragment.app.Fragment
import androidx.fragment.app.activityViewModels
import androidx.recyclerview.widget.LinearLayoutManager
import com.example.expense.R
import com.example.expense.data.Constants
import com.example.expense.data.ExpenseTransaction
import com.example.expense.databinding.FragmentTransactionsBinding
import com.example.expense.viewmodel.MainViewModel
import com.google.android.material.dialog.MaterialAlertDialogBuilder

class TransactionsFragment : Fragment() {

    private var _b: FragmentTransactionsBinding? = null
    private val b get() = _b!!
    private val vm: MainViewModel by activityViewModels()

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View {
        _b = FragmentTransactionsBinding.inflate(inflater, container, false)
        return b.root
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        // Start with clean filters every time this tab opens
        vm.query.value = ""
        vm.typeFilter.value = "ALL"
        vm.categoryFilter.value = "All"

        val adapter = TransactionAdapter(
            onEdit = { startActivity(AddTransactionActivity.intent(requireContext(), it.type, it.id)) },
            onDelete = { confirmDelete(it) }
        )
        b.rvTransactions.layoutManager = LinearLayoutManager(requireContext())
        b.rvTransactions.adapter = adapter

        vm.filtered.observe(viewLifecycleOwner) { list ->
            adapter.submitList(list)
            b.tvEmpty.visibility = if (list.isEmpty()) View.VISIBLE else View.GONE
            b.tvEmpty.text = if (vm.allTransactions.value.isNullOrEmpty())
                "No transactions yet.\nAdd one from the Home tab."
            else "No transactions match your search."
        }

        // Search box
        b.etSearch.doAfterTextChanged { vm.query.value = it.toString() }

        // Income / Expense / All chips
        b.chipGroup.setOnCheckedStateChangeListener { _, ids ->
            vm.typeFilter.value = when (ids.firstOrNull()) {
                R.id.chipIncome -> Constants.INCOME
                R.id.chipExpense -> Constants.EXPENSE
                else -> "ALL"
            }
        }

        // Category dropdown
        vm.allCategoryNames.observe(viewLifecycleOwner) { names ->
            val items = listOf("All") + names
            b.actCategory.setAdapter(ArrayAdapter(requireContext(), android.R.layout.simple_list_item_1, items))
            b.actCategory.setText(vm.categoryFilter.value, false)
            b.actCategory.setOnItemClickListener { _, _, pos, _ -> vm.categoryFilter.value = items[pos] }
        }
    }

    private fun confirmDelete(t: ExpenseTransaction) {
        MaterialAlertDialogBuilder(requireContext())
            .setTitle("Delete transaction?")
            .setMessage("This cannot be undone.")
            .setPositiveButton("Delete") { _, _ ->
                vm.delete(t)
                Toast.makeText(requireContext(), "Deleted", Toast.LENGTH_SHORT).show()
            }
            .setNegativeButton("Cancel", null)
            .show()
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _b = null
    }
}
```

### `ui/StatsFragment.kt`
```kotlin
package com.example.expense.ui

import android.content.res.ColorStateList
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.fragment.app.activityViewModels
import com.example.expense.R
import com.example.expense.data.Constants
import com.example.expense.data.ExpenseTransaction
import com.example.expense.data.Utils
import com.example.expense.databinding.FragmentStatsBinding
import com.example.expense.databinding.ItemLegendBinding
import com.example.expense.viewmodel.MainViewModel
import java.text.SimpleDateFormat
import java.util.Date
import java.util.Locale
import kotlin.math.roundToInt

class StatsFragment : Fragment() {

    private var _b: FragmentStatsBinding? = null
    private val b get() = _b!!
    private val vm: MainViewModel by activityViewModels()

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View {
        _b = FragmentStatsBinding.inflate(inflater, container, false)
        return b.root
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        // Restore the previously selected period, then listen for changes
        when (vm.period.value) {
            1 -> b.chipWeek.isChecked = true
            2 -> b.chipMonth.isChecked = true
            else -> b.chipDay.isChecked = true
        }
        b.chipPeriod.setOnCheckedStateChangeListener { _, ids ->
            vm.period.value = when (ids.firstOrNull()) {
                R.id.chipWeek -> 1
                R.id.chipMonth -> 2
                else -> 0
            }
        }
        vm.periodTransactions.observe(viewLifecycleOwner) { showPeriod(it) }
        vm.last7Days.observe(viewLifecycleOwner) { showLast7Days(it) }
    }

    private fun showPeriod(list: List<ExpenseTransaction>) {
        b.tvPeriodLabel.text = when (vm.period.value) {
            1 -> "This week (Mon - Sun)"
            2 -> "This month"
            else -> "Today"
        }
        val expenses = list.filter { it.type == Constants.EXPENSE }
        val totalExpense = expenses.sumOf { it.amount }
        val totalIncome = list.filter { it.type == Constants.INCOME }.sumOf { it.amount }

        b.tvExpense.text = Utils.money(totalExpense)
        b.tvIncome.text = Utils.money(totalIncome)
        b.tvCount.text = list.size.toString()

        // Group expenses by category and sort largest first
        val totals = expenses.groupBy { it.category }
            .map { (cat, items) -> cat to items.sumOf { it.amount } }
            .sortedByDescending { it.second }

        b.pieChart.setData(totals)
        b.legendContainer.removeAllViews()
        totals.forEach { (cat, amount) ->
            val row = ItemLegendBinding.inflate(layoutInflater, b.legendContainer, false)
            row.viewDot.backgroundTintList = ColorStateList.valueOf(Utils.categoryColor(cat))
            row.tvLegendName.text = cat
            row.tvLegendValue.text = "${Utils.money(amount)} (${(amount / totalExpense * 100).roundToInt()}%)"
            b.legendContainer.addView(row.root)
        }
        val empty = totals.isEmpty()
        b.pieChart.visibility = if (empty) View.GONE else View.VISIBLE
        b.tvPieEmpty.visibility = if (empty) View.VISIBLE else View.GONE
    }

    private fun showLast7Days(list: List<ExpenseTransaction>) {
        val byDay = list.filter { it.type == Constants.EXPENSE }
            .groupBy { it.date }
            .mapValues { (_, items) -> items.sumOf { it.amount } }
        val fmt = SimpleDateFormat("EEE", Locale.getDefault())
        val data = (6 downTo 0).map { i ->
            val day = Utils.dayStartAgo(i)
            fmt.format(Date(day)) to (byDay[day] ?: 0.0)
        }
        b.barChart.setData(data)
        val hasData = data.any { it.second > 0 }
        b.barChart.visibility = if (hasData) View.VISIBLE else View.GONE
        b.tvBarEmpty.visibility = if (hasData) View.GONE else View.VISIBLE
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _b = null
    }
}
```

### `ui/SettingsFragment.kt`
```kotlin
package com.example.expense.ui

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ArrayAdapter
import android.widget.Toast
import androidx.appcompat.app.AppCompatDelegate
import androidx.fragment.app.Fragment
import androidx.fragment.app.activityViewModels
import com.example.expense.data.Utils
import com.example.expense.databinding.FragmentSettingsBinding
import com.example.expense.viewmodel.MainViewModel
import com.google.android.material.dialog.MaterialAlertDialogBuilder
import java.util.Locale

class SettingsFragment : Fragment() {

    private var _b: FragmentSettingsBinding? = null
    private val b get() = _b!!
    private val vm: MainViewModel by activityViewModels()

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View {
        _b = FragmentSettingsBinding.inflate(inflater, container, false)
        return b.root
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        // ----- Profile -----
        vm.userName.observe(viewLifecycleOwner) {
            if (b.etName.text.toString() != it) b.etName.setText(it)
        }
        b.btnSaveName.setOnClickListener {
            val name = b.etName.text.toString().trim()
            if (name.isEmpty()) {
                b.tilName.error = "Name cannot be empty"
            } else {
                b.tilName.error = null
                vm.savePref("name", name)
                toast("Profile saved")
            }
        }

        // ----- Monthly budget -----
        vm.monthBudget.observe(viewLifecycleOwner) {
            if (it > 0) b.etBudget.setText(String.format(Locale.US, "%.0f", it))
        }
        b.btnSaveBudget.setOnClickListener {
            val amount = b.etBudget.text.toString().trim().toDoubleOrNull()
            if (amount == null || amount <= 0) {
                b.tilBudget.error = "Enter a valid amount"
            } else {
                b.tilBudget.error = null
                vm.setBudget(amount)
                toast("Budget saved")
            }
        }

        // ----- Currency -----
        val currencies = listOf("₹ INR", "$ USD", "€ EUR", "£ GBP")
        b.actCurrency.setAdapter(ArrayAdapter(requireContext(), android.R.layout.simple_list_item_1, currencies))
        b.actCurrency.setText(currencies.firstOrNull { it.startsWith(Utils.currency) } ?: currencies[0], false)
        b.actCurrency.setOnItemClickListener { _, _, pos, _ ->
            val symbol = currencies[pos].substring(0, 1)
            if (symbol != Utils.currency) {
                Utils.currency = symbol
                vm.savePref("currency", symbol)
                requireActivity().recreate()          // redraw all screens with the new symbol
            }
        }

        // ----- Theme (light / dark / system) -----
        vm.themeMode.observe(viewLifecycleOwner) {
            when (it) {
                "light" -> b.rbLight.isChecked = true
                "dark" -> b.rbDark.isChecked = true
                else -> b.rbSystem.isChecked = true
            }
        }
        b.rbLight.setOnClickListener { setTheme("light") }
        b.rbDark.setOnClickListener { setTheme("dark") }
        b.rbSystem.setOnClickListener { setTheme("system") }

        // ----- Data -----
        b.btnSample.setOnClickListener {
            vm.loadSampleData()
            toast("Sample data added")
        }
        b.btnClear.setOnClickListener {
            MaterialAlertDialogBuilder(requireContext())
                .setTitle("Clear all data?")
                .setMessage("All transactions and the budget will be deleted permanently.")
                .setPositiveButton("Delete") { _, _ ->
                    vm.clearAll()
                    toast("All data cleared")
                }
                .setNegativeButton("Cancel", null)
                .show()
        }
    }

    private fun setTheme(mode: String) {
        vm.savePref("theme", mode)
        AppCompatDelegate.setDefaultNightMode(Utils.nightMode(mode))   // recreates the activity
    }

    private fun toast(msg: String) = Toast.makeText(requireContext(), msg, Toast.LENGTH_SHORT).show()

    override fun onDestroyView() {
        super.onDestroyView()
        _b = null
    }
}
```

### `ui/charts/PieChartView.kt`
```kotlin
package com.example.expense.ui.charts

import android.content.Context
import android.graphics.Canvas
import android.graphics.Paint
import android.graphics.RectF
import android.util.AttributeSet
import android.view.View
import com.example.expense.data.Utils

/** Donut chart drawn with Canvas. Data = list of (category name, amount). */
class PieChartView @JvmOverloads constructor(
    context: Context, attrs: AttributeSet? = null, defStyle: Int = 0
) : View(context, attrs, defStyle) {

    private var items: List<Pair<String, Double>> = emptyList()
    private val paint = Paint(Paint.ANTI_ALIAS_FLAG).apply { style = Paint.Style.STROKE }
    private val rect = RectF()

    fun setData(data: List<Pair<String, Double>>) {
        items = data
        invalidate()          // ask Android to redraw
    }

    override fun onDraw(canvas: Canvas) {
        val total = items.sumOf { it.second }
        if (total <= 0.0) return

        val size = minOf(width, height).toFloat()
        val stroke = size * 0.2f
        paint.strokeWidth = stroke
        val left = (width - size) / 2f + stroke / 2f
        val top = (height - size) / 2f + stroke / 2f
        rect.set(left, top, left + size - stroke, top + size - stroke)

        var startAngle = -90f
        items.forEach { (name, value) ->
            val sweep = (value / total * 360.0).toFloat()
            paint.color = Utils.categoryColor(name)
            canvas.drawArc(rect, startAngle, sweep, false, paint)
            startAngle += sweep
        }
    }
}
```

### `ui/charts/BarChartView.kt`
```kotlin
package com.example.expense.ui.charts

import android.content.Context
import android.graphics.Canvas
import android.graphics.Paint
import android.util.AttributeSet
import android.util.TypedValue
import android.view.View
import androidx.core.content.ContextCompat
import com.example.expense.R
import java.util.Locale

/** Simple bar chart. Data = list of (label, value). */
class BarChartView @JvmOverloads constructor(
    context: Context, attrs: AttributeSet? = null, defStyle: Int = 0
) : View(context, attrs, defStyle) {

    private var items: List<Pair<String, Double>> = emptyList()

    private val barPaint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        color = ContextCompat.getColor(context, R.color.primary)
    }
    private val textPaint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        color = ContextCompat.getColor(context, R.color.text_secondary)
        textAlign = Paint.Align.CENTER
        textSize = TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_SP, 11f, resources.displayMetrics)
    }

    fun setData(data: List<Pair<String, Double>>) {
        items = data
        invalidate()
    }

    private fun short(v: Double) =
        if (v >= 1000) String.format(Locale.US, "%.1fk", v / 1000) else v.toInt().toString()

    override fun onDraw(canvas: Canvas) {
        val max = items.maxOfOrNull { it.second } ?: 0.0
        if (items.isEmpty() || max <= 0.0) return

        val labelSpace = textPaint.textSize * 2f      // room for day names at the bottom
        val topSpace = textPaint.textSize * 1.6f      // room for the value above the tallest bar
        val chartHeight = height - labelSpace - topSpace
        val slot = width.toFloat() / items.size
        val barWidth = slot * 0.5f

        items.forEachIndexed { i, (label, value) ->
            val cx = slot * i + slot / 2f
            val barHeight = (value / max * chartHeight).toFloat()
            val top = topSpace + chartHeight - barHeight
            if (value > 0) {
                canvas.drawRoundRect(cx - barWidth / 2, top, cx + barWidth / 2, topSpace + chartHeight, 12f, 12f, barPaint)
                canvas.drawText(short(value), cx, top - 8f, textPaint)
            }
            canvas.drawText(label, cx, height - textPaint.textSize * 0.6f, textPaint)
        }
    }
}
```

---

# PART 5 – LAYOUT XML (`app/src/main/res/layout/`)

### `activity_splash.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/bg_splash"
    android:gravity="center"
    android:orientation="vertical">

    <ImageView
        android:layout_width="96dp"
        android:layout_height="96dp"
        android:contentDescription="App logo"
        android:src="@drawable/ic_wallet"
        android:tint="@color/white" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:text="Daily Expense Tracker"
        android:textColor="@color/white"
        android:textSize="26sp"
        android:textStyle="bold" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="6dp"
        android:text="Track smart. Spend wise."
        android:textColor="#CCFFFFFF"
        android:textSize="14sp" />
</LinearLayout>
```

### `activity_main.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/bg"
    android:orientation="vertical">

    <FrameLayout
        android:id="@+id/container"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1" />

    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/bottomNav"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:labelVisibilityMode="labeled"
        app:menu="@menu/bottom_nav" />
</LinearLayout>
```

### `activity_add_transaction.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/bg"
    android:orientation="vertical">

    <com.google.android.material.appbar.MaterialToolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="@color/card"
        app:navigationIcon="@drawable/ic_back"
        app:navigationIconTint="@color/text_primary"
        app:title="Add Expense"
        app:titleTextColor="@color/text_primary" />

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:fillViewport="true">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="16dp">

            <com.google.android.material.textfield.TextInputLayout
                android:id="@+id/tilAmount"
                style="@style/AppInput"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:hint="Amount">

                <com.google.android.material.textfield.TextInputEditText
                    android:id="@+id/etAmount"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:inputType="numberDecimal"
                    android:maxLength="12" />
            </com.google.android.material.textfield.TextInputLayout>

            <com.google.android.material.textfield.TextInputLayout
                android:id="@+id/tilCategory"
                style="@style/AppDropdown"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="12dp"
                android:hint="Category">

                <com.google.android.material.textfield.MaterialAutoCompleteTextView
                    android:id="@+id/actCategory"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:inputType="none" />
            </com.google.android.material.textfield.TextInputLayout>

            <com.google.android.material.textfield.TextInputLayout
                android:id="@+id/tilDate"
                style="@style/AppInput"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="12dp"
                android:hint="Date">

                <com.google.android.material.textfield.TextInputEditText
                    android:id="@+id/etDate"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:cursorVisible="false"
                    android:focusable="false"
                    android:inputType="none" />
            </com.google.android.material.textfield.TextInputLayout>

            <com.google.android.material.textfield.TextInputLayout
                android:id="@+id/tilPayment"
                style="@style/AppDropdown"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="12dp"
                android:hint="Payment method">

                <com.google.android.material.textfield.MaterialAutoCompleteTextView
                    android:id="@+id/actPayment"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:inputType="none" />
            </com.google.android.material.textfield.TextInputLayout>

            <com.google.android.material.textfield.TextInputLayout
                android:id="@+id/tilDescription"
                style="@style/AppInput"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="12dp"
                android:hint="Description (optional)"
                app:counterEnabled="true"
                app:counterMaxLength="100">

                <com.google.android.material.textfield.TextInputEditText
                    android:id="@+id/etDescription"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:inputType="textCapSentences|textMultiLine"
                    android:maxLength="100"
                    android:minLines="2" />
            </com.google.android.material.textfield.TextInputLayout>

            <com.google.android.material.button.MaterialButton
                android:id="@+id/btnSave"
                style="@style/AppButton"
                android:layout_width="match_parent"
                android:layout_height="56dp"
                android:layout_marginTop="20dp"
                android:text="Save"
                android:textSize="16sp" />
        </LinearLayout>
    </ScrollView>
</LinearLayout>
```

### `item_transaction.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<com.google.android.material.card.MaterialCardView xmlns:android="http://schemas.android.com/apk/res/android"
    style="@style/AppCard"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginVertical="4dp"
    android:clickable="true"
    android:focusable="true">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center_vertical"
        android:orientation="horizontal"
        android:padding="12dp">

        <TextView
            android:id="@+id/tvInitial"
            android:layout_width="44dp"
            android:layout_height="44dp"
            android:background="@drawable/bg_circle"
            android:gravity="center"
            android:textColor="@color/white"
            android:textSize="18sp"
            android:textStyle="bold" />

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_marginStart="12dp"
            android:layout_weight="1"
            android:orientation="vertical">

            <TextView
                android:id="@+id/tvCategory"
                style="@style/TextTitle"
                android:layout_width="match_parent"
                android:layout_height="wrap_content" />

            <TextView
                android:id="@+id/tvInfo"
                style="@style/TextCaption"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:ellipsize="end"
                android:maxLines="1" />
        </LinearLayout>

        <TextView
            android:id="@+id/tvAmount"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:textSize="15sp"
            android:textStyle="bold" />

        <ImageButton
            android:id="@+id/btnEdit"
            android:layout_width="40dp"
            android:layout_height="40dp"
            android:background="?attr/selectableItemBackgroundBorderless"
            android:contentDescription="Edit"
            android:src="@drawable/ic_edit"
            android:tint="@color/text_secondary" />

        <ImageButton
            android:id="@+id/btnDelete"
            android:layout_width="40dp"
            android:layout_height="40dp"
            android:background="?attr/selectableItemBackgroundBorderless"
            android:contentDescription="Delete"
            android:src="@drawable/ic_delete"
            android:tint="@color/expense" />
    </LinearLayout>
</com.google.android.material.card.MaterialCardView>
```

### `item_legend.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="center_vertical"
    android:orientation="horizontal"
    android:paddingVertical="4dp">

    <View
        android:id="@+id/viewDot"
        android:layout_width="12dp"
        android:layout_height="12dp"
        android:background="@drawable/bg_circle" />

    <TextView
        android:id="@+id/tvLegendName"
        style="@style/TextBody"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="10dp"
        android:layout_weight="1" />

    <TextView
        android:id="@+id/tvLegendValue"
        style="@style/TextCaption"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
</LinearLayout>
```

### `fragment_home.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.core.widget.NestedScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fillViewport="true">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:id="@+id/tvGreeting"
            style="@style/TextHeading"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Hello" />

        <TextView
            style="@style/TextCaption"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Here is your financial overview" />

        <!-- Balance card -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:background="@drawable/bg_gradient_card"
            android:orientation="vertical"
            android:padding="20dp">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Total Balance"
                android:textColor="#CCFFFFFF"
                android:textSize="14sp" />

            <TextView
                android:id="@+id/tvBalance"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginTop="4dp"
                android:text="0.00"
                android:textColor="@color/white"
                android:textSize="32sp"
                android:textStyle="bold" />

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="18dp"
                android:orientation="horizontal">

                <LinearLayout
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:orientation="vertical">

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Income"
                        android:textColor="#CCFFFFFF"
                        android:textSize="13sp" />

                    <TextView
                        android:id="@+id/tvIncome"
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="0.00"
                        android:textColor="@color/white"
                        android:textSize="17sp"
                        android:textStyle="bold" />
                </LinearLayout>

                <LinearLayout
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:orientation="vertical">

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Expenses"
                        android:textColor="#CCFFFFFF"
                        android:textSize="13sp" />

                    <TextView
                        android:id="@+id/tvExpense"
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="0.00"
                        android:textColor="@color/white"
                        android:textSize="17sp"
                        android:textStyle="bold" />
                </LinearLayout>
            </LinearLayout>
        </LinearLayout>

        <!-- Quick add buttons -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:orientation="horizontal">

            <com.google.android.material.button.MaterialButton
                android:id="@+id/btnAddExpense"
                style="@style/AppButton"
                android:layout_width="0dp"
                android:layout_height="52dp"
                android:layout_marginEnd="8dp"
                android:layout_weight="1"
                android:text="Add Expense"
                android:textColor="@color/white"
                app:backgroundTint="@color/expense" />

            <com.google.android.material.button.MaterialButton
                android:id="@+id/btnAddIncome"
                style="@style/AppButton"
                android:layout_width="0dp"
                android:layout_height="52dp"
                android:layout_marginStart="8dp"
                android:layout_weight="1"
                android:text="Add Income"
                android:textColor="@color/white"
                app:backgroundTint="@color/income" />
        </LinearLayout>

        <!-- Budget card -->
        <com.google.android.material.card.MaterialCardView
            style="@style/AppCard"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="16dp">

                <TextView
                    style="@style/TextTitle"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Monthly Budget" />

                <TextView
                    android:id="@+id/tvBudgetInfo"
                    style="@style/TextCaption"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="4dp" />

                <com.google.android.material.progressindicator.LinearProgressIndicator
                    android:id="@+id/progressBudget"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="10dp"
                    android:max="100"
                    app:trackColor="@color/divider"
                    app:trackCornerRadius="8dp"
                    app:trackThickness="10dp" />

                <TextView
                    android:id="@+id/tvBudgetWarning"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="8dp"
                    android:textSize="13sp"
                    android:textStyle="bold"
                    android:visibility="gone" />
            </LinearLayout>
        </com.google.android.material.card.MaterialCardView>

        <!-- Recent transactions -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="20dp"
            android:layout_marginBottom="8dp"
            android:gravity="center_vertical"
            android:orientation="horizontal">

            <TextView
                style="@style/TextTitle"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="Recent Transactions" />

            <TextView
                android:id="@+id/tvSeeAll"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:padding="4dp"
                android:text="See all"
                android:textColor="@color/primary"
                android:textSize="14sp"
                android:textStyle="bold" />
        </LinearLayout>

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/rvRecent"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:nestedScrollingEnabled="false" />

        <TextView
            android:id="@+id/tvEmpty"
            style="@style/TextCaption"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:padding="24dp"
            android:text="No transactions yet.\nTap Add Expense or Add Income to start."
            android:textSize="14sp"
            android:visibility="gone" />
    </LinearLayout>
</androidx.core.widget.NestedScrollView>
```

### `fragment_transactions.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        style="@style/TextHeading"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Transactions" />

    <com.google.android.material.textfield.TextInputLayout
        style="@style/AppInput"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="12dp"
        android:hint="Search by category, note, payment, amount"
        app:startIconDrawable="@drawable/ic_search">

        <com.google.android.material.textfield.TextInputEditText
            android:id="@+id/etSearch"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:imeOptions="actionSearch"
            android:inputType="text"
            android:maxLines="1" />
    </com.google.android.material.textfield.TextInputLayout>

    <com.google.android.material.chip.ChipGroup
        android:id="@+id/chipGroup"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        app:selectionRequired="true"
        app:singleSelection="true">

        <com.google.android.material.chip.Chip
            android:id="@+id/chipAll"
            style="@style/Widget.Material3.Chip.Filter"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:checked="true"
            android:text="All" />

        <com.google.android.material.chip.Chip
            android:id="@+id/chipIncome"
            style="@style/Widget.Material3.Chip.Filter"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Income" />

        <com.google.android.material.chip.Chip
            android:id="@+id/chipExpense"
            style="@style/Widget.Material3.Chip.Filter"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Expense" />
    </com.google.android.material.chip.ChipGroup>

    <com.google.android.material.textfield.TextInputLayout
        style="@style/AppDropdown"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="4dp"
        android:hint="Category">

        <com.google.android.material.textfield.MaterialAutoCompleteTextView
            android:id="@+id/actCategory"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:inputType="none"
            android:text="All" />
    </com.google.android.material.textfield.TextInputLayout>

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginTop="8dp"
        android:layout_weight="1">

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/rvTransactions"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

        <TextView
            android:id="@+id/tvEmpty"
            style="@style/TextCaption"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:gravity="center"
            android:textSize="14sp"
            android:visibility="gone" />
    </FrameLayout>
</LinearLayout>
```

### `fragment_stats.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fillViewport="true">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            style="@style/TextHeading"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Statistics" />

        <com.google.android.material.chip.ChipGroup
            android:id="@+id/chipPeriod"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            app:selectionRequired="true"
            app:singleSelection="true">

            <com.google.android.material.chip.Chip
                android:id="@+id/chipDay"
                style="@style/Widget.Material3.Chip.Filter"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:checked="true"
                android:text="Daily" />

            <com.google.android.material.chip.Chip
                android:id="@+id/chipWeek"
                style="@style/Widget.Material3.Chip.Filter"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Weekly" />

            <com.google.android.material.chip.Chip
                android:id="@+id/chipMonth"
                style="@style/Widget.Material3.Chip.Filter"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Monthly" />
        </com.google.android.material.chip.ChipGroup>

        <!-- Summary card -->
        <com.google.android.material.card.MaterialCardView
            style="@style/AppCard"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="16dp">

                <TextView
                    android:id="@+id/tvPeriodLabel"
                    style="@style/TextTitle"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content" />

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="12dp"
                    android:orientation="horizontal">

                    <LinearLayout
                        android:layout_width="0dp"
                        android:layout_height="wrap_content"
                        android:layout_weight="1"
                        android:orientation="vertical">

                        <TextView
                            style="@style/TextCaption"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:text="Expenses" />

                        <TextView
                            android:id="@+id/tvExpense"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:textColor="@color/expense"
                            android:textSize="16sp"
                            android:textStyle="bold" />
                    </LinearLayout>

                    <LinearLayout
                        android:layout_width="0dp"
                        android:layout_height="wrap_content"
                        android:layout_weight="1"
                        android:orientation="vertical">

                        <TextView
                            style="@style/TextCaption"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:text="Income" />

                        <TextView
                            android:id="@+id/tvIncome"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:textColor="@color/income"
                            android:textSize="16sp"
                            android:textStyle="bold" />
                    </LinearLayout>

                    <LinearLayout
                        android:layout_width="0dp"
                        android:layout_height="wrap_content"
                        android:layout_weight="0.7"
                        android:orientation="vertical">

                        <TextView
                            style="@style/TextCaption"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:text="Count" />

                        <TextView
                            android:id="@+id/tvCount"
                            style="@style/TextTitle"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content" />
                    </LinearLayout>
                </LinearLayout>
            </LinearLayout>
        </com.google.android.material.card.MaterialCardView>

        <!-- Donut chart card -->
        <com.google.android.material.card.MaterialCardView
            style="@style/AppCard"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="16dp">

                <TextView
                    style="@style/TextTitle"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Spending by category" />

                <com.example.expense.ui.charts.PieChartView
                    android:id="@+id/pieChart"
                    android:layout_width="match_parent"
                    android:layout_height="220dp"
                    android:layout_marginTop="8dp" />

                <TextView
                    android:id="@+id/tvPieEmpty"
                    style="@style/TextCaption"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:gravity="center"
                    android:padding="24dp"
                    android:text="No expenses in this period"
                    android:textSize="14sp"
                    android:visibility="gone" />

                <LinearLayout
                    android:id="@+id/legendContainer"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="8dp"
                    android:orientation="vertical" />
            </LinearLayout>
        </com.google.android.material.card.MaterialCardView>

        <!-- Bar chart card -->
        <com.google.android.material.card.MaterialCardView
            style="@style/AppCard"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="16dp">

                <TextView
                    style="@style/TextTitle"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Expenses - last 7 days" />

                <com.example.expense.ui.charts.BarChartView
                    android:id="@+id/barChart"
                    android:layout_width="match_parent"
                    android:layout_height="200dp"
                    android:layout_marginTop="8dp" />

                <TextView
                    android:id="@+id/tvBarEmpty"
                    style="@style/TextCaption"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:gravity="center"
                    android:padding="24dp"
                    android:text="No expenses in the last 7 days"
                    android:textSize="14sp"
                    android:visibility="gone" />
            </LinearLayout>
        </com.google.android.material.card.MaterialCardView>
    </LinearLayout>
</ScrollView>
```

### `fragment_settings.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fillViewport="true">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            style="@style/TextHeading"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Settings" />

        <!-- Profile -->
        <com.google.android.material.card.MaterialCardView
            style="@style/AppCard"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="16dp">

                <TextView
                    style="@style/TextTitle"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Profile" />

                <com.google.android.material.textfield.TextInputLayout
                    android:id="@+id/tilName"
                    style="@style/AppInput"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="10dp"
                    android:hint="Your name">

                    <com.google.android.material.textfield.TextInputEditText
                        android:id="@+id/etName"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:inputType="textPersonName|textCapWords"
                        android:maxLength="30"
                        android:maxLines="1" />
                </com.google.android.material.textfield.TextInputLayout>

                <com.google.android.material.button.MaterialButton
                    android:id="@+id/btnSaveName"
                    style="@style/AppButton"
                    android:layout_width="match_parent"
                    android:layout_height="52dp"
                    android:layout_marginTop="10dp"
                    android:text="Save profile" />
            </LinearLayout>
        </com.google.android.material.card.MaterialCardView>

        <!-- Budget -->
        <com.google.android.material.card.MaterialCardView
            style="@style/AppCard"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="16dp">

                <TextView
                    style="@style/TextTitle"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Monthly Budget" />

                <TextView
                    style="@style/TextCaption"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Spending limit for this month. You get a warning at 80%." />

                <com.google.android.material.textfield.TextInputLayout
                    android:id="@+id/tilBudget"
                    style="@style/AppInput"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="10dp"
                    android:hint="Budget amount">

                    <com.google.android.material.textfield.TextInputEditText
                        android:id="@+id/etBudget"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:inputType="numberDecimal"
                        android:maxLength="12" />
                </com.google.android.material.textfield.TextInputLayout>

                <com.google.android.material.button.MaterialButton
                    android:id="@+id/btnSaveBudget"
                    style="@style/AppButton"
                    android:layout_width="match_parent"
                    android:layout_height="52dp"
                    android:layout_marginTop="10dp"
                    android:text="Save budget" />
            </LinearLayout>
        </com.google.android.material.card.MaterialCardView>

        <!-- Preferences -->
        <com.google.android.material.card.MaterialCardView
            style="@style/AppCard"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="16dp">

                <TextView
                    style="@style/TextTitle"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Preferences" />

                <com.google.android.material.textfield.TextInputLayout
                    style="@style/AppDropdown"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="10dp"
                    android:hint="Currency">

                    <com.google.android.material.textfield.MaterialAutoCompleteTextView
                        android:id="@+id/actCurrency"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:inputType="none" />
                </com.google.android.material.textfield.TextInputLayout>

                <TextView
                    style="@style/TextCaption"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="14dp"
                    android:text="Theme" />

                <RadioGroup
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="vertical">

                    <RadioButton
                        android:id="@+id/rbLight"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="Light mode"
                        android:textColor="@color/text_primary" />

                    <RadioButton
                        android:id="@+id/rbDark"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="Dark mode"
                        android:textColor="@color/text_primary" />

                    <RadioButton
                        android:id="@+id/rbSystem"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:checked="true"
                        android:text="Follow system"
                        android:textColor="@color/text_primary" />
                </RadioGroup>
            </LinearLayout>
        </com.google.android.material.card.MaterialCardView>

        <!-- Data -->
        <com.google.android.material.card.MaterialCardView
            style="@style/AppCard"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="16dp">

                <TextView
                    style="@style/TextTitle"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Data" />

                <com.google.android.material.button.MaterialButton
                    android:id="@+id/btnSample"
                    style="@style/Widget.Material3.Button.OutlinedButton"
                    android:layout_width="match_parent"
                    android:layout_height="52dp"
                    android:layout_marginTop="10dp"
                    android:text="Load sample data (also sets a 20,000 budget)" />

                <com.google.android.material.button.MaterialButton
                    android:id="@+id/btnClear"
                    style="@style/Widget.Material3.Button.OutlinedButton"
                    android:layout_width="match_parent"
                    android:layout_height="52dp"
                    android:layout_marginTop="8dp"
                    android:text="Clear all data"
                    android:textColor="@color/expense"
                    app:strokeColor="@color/expense" />
            </LinearLayout>
        </com.google.android.material.card.MaterialCardView>

        <TextView
            style="@style/TextCaption"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:gravity="center"
            android:text="Daily Expense Tracker v1.0 • Works offline" />
    </LinearLayout>
</ScrollView>
```

---

# PART 6 – QUICK REVIEW CHECKLIST BEFORE RUNNING

1. Package of every Kotlin file is `com.example.expense` (or `.data`, `.database`, `.repository`, `.viewmodel`, `.ui`, `.ui.charts`).
2. Deleted the wizard's `values/themes.xml`, `values-night/themes.xml` and its own `colors.xml` (ours replace them).
3. Gradle wrapper = 8.7, JDK 17 (Android Studio's built-in JDK works), SDK Platform 34 installed.
4. After pasting: **File → Sync Project with Gradle Files**, then **Build → Rebuild Project**.
5. Run, then open **Settings → Load sample data**.

## Next steps if you get an error
Send me the exact error text from the **Build** tab (and the file name), and I will give only the fixed file.
