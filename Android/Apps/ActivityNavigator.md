## SIMPLE APP

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val btn = findViewById<Button>(R.id.main_activity_btn)

            btn.setOnClickListener {
                val intent = Intent(this, SecondActivity::class.java)
                startActivity(intent)
                finish()
            }
        }
    }
```

```kotlin
    class SecondActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_second)

            val btn = findViewById<Button>(R.id.second_activity_btn)

            btn.setOnClickListener {
                val intent = Intent(this, MainActivity::class.java)
                startActivity(intent)
                finish()
            }
        }
    }
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/main_activity_btn"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:text="Go to second activity"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SecondActivity">

    <Button
        android:id="@+id/second_activity_btn"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:text="Go to main activity"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

**Note**: add `<activity android:name=".SecondActivity"></activity>` in `AndroidManifest.xml`

-   need to do above manually if you create new activity class manually
-   you can create new activity by `RightClickOnPackage->New->Activity->Empty Activity`. In this case you dont need to update `AndroidManifest.xml` manually

## NAVIGATOR WITH DATA PASSING

```xml
//res/layout/a_food_item.xml
<?xml version="1.0" encoding="utf-8"?>
<com.google.android.material.card.MaterialCardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="16dp"
    android:elevation="10dp">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <ImageView
            android:id="@+id/a_food_item_image"
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:scaleType="centerCrop"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:srcCompat="@drawable/image1"
            tools:layout_editor_absoluteX="27dp" />

        <TextView
            android:id="@+id/a_food_item_text"
            android:layout_width="200dp"
            android:layout_height="wrap_content"
            android:layout_marginTop="40dp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.392"
            app:layout_constraintStart_toEndOf="@id/a_food_item_image"
            android:text="Food!!!"
            android:textSize="24sp"
            app:layout_constraintTop_toTopOf="parent" />

    </androidx.constraintlayout.widget.ConstraintLayout>
</com.google.android.material.card.MaterialCardView>
```

```xml
//res/layout/activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/main_recycle_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="vertical"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager" />

</FrameLayout>
```

```xml
//res/layout/activity_detailed.xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".DetailedActivity">

    <ImageView
        android:id="@+id/detail_image_id"
        android:layout_width="match_parent"
        android:layout_height="200sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/image1"
        android:scaleType="centerCrop"
        />

    <TextView
        android:id="@+id/detail_text_id"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:text="Food 1"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/detail_image_id"
        android:textSize="24sp"
        android:layout_margin="20sp"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

```kotlin
//java/com/example/recyclerfoodapp/FoodItem.kt

    package com.example.recyclerfoodapp

    data class FoodItem(val imageId: Int, val foodName: String)

```

```kotlin
//java/com/example/recyclerfoodapp/MainActivity.kt

class MainActivity : AppCompatActivity() {

    private lateinit var foodList: List<FoodItem>

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val recycleView:RecyclerView = findViewById(R.id.main_recycle_view)

        recycleView.setHasFixedSize(true)

        recycleView.layoutManager = LinearLayoutManager(this)

        foodList = listOf<FoodItem>(
            FoodItem( R.drawable.image1, "Candy 1"),
            FoodItem( R.drawable.image2, "Candy 2"),
            FoodItem( R.drawable.image3, "Candy 3"),
            FoodItem( R.drawable.image4, "Candy 4"),
            FoodItem( R.drawable.image5, "Candy 5"),
            FoodItem( R.drawable.image6, "Candy 6"),
            FoodItem( R.drawable.image7, "Candy 7"),
            FoodItem( R.drawable.image8, "Candy 8"),
            FoodItem( R.drawable.image9, "Candy 9"),
            FoodItem( R.drawable.image10, "Candy 10")
        )

        val foodAdapter = FoodAdapter(this, foodList)
        recycleView.adapter = foodAdapter
    }
}
```

```kotlin
//java/com/example/recyclerfoodapp/FoodAdapter.kt

class FoodAdapter(val context: Context, private val foodList: List<FoodItem>) :
    RecyclerView.Adapter<FoodAdapter.ViewHolder>() {

    class ViewHolder(private val view: View) : RecyclerView.ViewHolder(view) {
        val aFoodImage: ImageView = view.findViewById(R.id.a_food_item_image)
        val aFoodText: TextView = view.findViewById(R.id.a_food_item_text)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val layout = LayoutInflater.from(parent.context).inflate(R.layout.a_food_item, parent, false)
        return ViewHolder(layout)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val item = foodList[position]
        holder.aFoodText.text = item.foodName.toString()
        holder.aFoodImage.setImageResource(item.imageId)

        holder.aFoodImage.setOnClickListener{
            val context = holder.itemView.context
            val intent = Intent(context, DetailedActivity::class.java)
            intent.putExtra("foodName", item.foodName)
            intent.putExtra("foodImage", item.imageId)
            context.startActivity(intent)
        }
    }

    override fun getItemCount(): Int {
        return foodList.size
    }
}
```

```kotlin
//java/com/example/recyclerfoodapp/DetailedActivity.kt

class DetailedActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_detailed)

        val foodName = intent?.extras?.getString("foodName").toString()
        val foodImage = intent?.extras?.getInt("foodImage").toString()

        if(intent != null){
            val textView: TextView = findViewById(R.id.detail_text_id)
            textView.text = foodName
            val imageView: ImageView = findViewById(R.id.detail_image_id)
            imageView.setImageResource(foodImage.toInt())
        }
    }
}
```
