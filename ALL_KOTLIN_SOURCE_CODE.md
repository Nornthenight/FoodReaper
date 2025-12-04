# FoodReaper - All Kotlin Source Code Files

## How to Use
Copy the content of each file below into your Android Studio project in the corresponding location.

---

## 1. MainActivity.kt
**Path:** `app/src/main/kotlin/com/foodreaper/MainActivity.kt`

```kotlin
package com.foodreaper

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.ui.Modifier
import androidx.lifecycle.ViewModelProvider
import com.foodreaper.ui.screens.HomeScreen
import com.foodreaper.ui.theme.FoodReaperTheme
import com.foodreaper.viewmodel.ProductViewModel

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val viewModel = ViewModelProvider(this).get(ProductViewModel::class.java)
        setContent {
            FoodReaperTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    HomeScreen(viewModel)
                }
            }
        }
    }
}
```

---

## 2. Product.kt
**Path:** `app/src/main/kotlin/com/foodreaper/model/Product.kt`

```kotlin
package com.foodreaper.model

import androidx.room.Entity
import androidx.room.PrimaryKey
import java.time.LocalDate

@Entity(tableName = "products")
data class Product(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    val name: String,
    val weight: Int,
    val expiryDate: LocalDate,
    val category: String,
    val dateAdded: LocalDate = LocalDate.now()
)
```

---

## 3. AppDatabase.kt
**Path:** `app/src/main/kotlin/com/foodreaper/database/AppDatabase.kt`

```kotlin
package com.foodreaper.database

import android.content.Context
import androidx.room.Database
import androidx.room.Room
import androidx.room.RoomDatabase
import androidx.room.TypeConverters
import com.foodreaper.model.Product

@Database(entities = [Product::class], version = 1, exportSchema = false)
@TypeConverters(Converters::class)
abstract class AppDatabase : RoomDatabase() {
    abstract fun productDao(): ProductDao

    companion object {
        @Volatile
        private var INSTANCE: AppDatabase? = null

        fun getDatabase(context: Context): AppDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    AppDatabase::class.java,
                    "foodreaper_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}
```

---

## 4. ProductDao.kt
**Path:** `app/src/main/kotlin/com/foodreaper/database/ProductDao.kt`

```kotlin
package com.foodreaper.database

import androidx.room.Dao
import androidx.room.Delete
import androidx.room.Insert
import androidx.room.Query
import androidx.room.Update
import com.foodreaper.model.Product
import java.time.LocalDate

@Dao
interface ProductDao {
    @Insert
    suspend fun insert(product: Product)

    @Update
    suspend fun update(product: Product)

    @Delete
    suspend fun delete(product: Product)

    @Query("SELECT * FROM products ORDER BY expiryDate ASC")
    suspend fun getAllProducts(): List<Product>

    @Query("SELECT * FROM products WHERE expiryDate <= :date")
    suspend fun getExpiringProducts(date: LocalDate): List<Product>

    @Query("SELECT * FROM products WHERE id = :id")
    suspend fun getProductById(id: Int): Product?
}
```

---

## 5. Converters.kt
**Path:** `app/src/main/kotlin/com/foodreaper/database/Converters.kt`

```kotlin
package com.foodreaper.database

import androidx.room.TypeConverter
import java.time.LocalDate

class Converters {
    @TypeConverter
    fun fromLocalDate(value: LocalDate?): String? = value?.toString()

    @TypeConverter
    fun toLocalDate(value: String?): LocalDate? = value?.let { LocalDate.parse(it) }
}
```
