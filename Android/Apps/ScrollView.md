
list app (Affirmation)
----------------------

```xml
res/layout/activity_main.xml

    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">


        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recycler_view"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layoutManager="LinearLayoutManager"
            android:scrollbars="vertical"/>
    </FrameLayout>
```
```xml
res/layout/list_item.xml

    <?xml version="1.0" encoding="utf-8"?>
    <TextView xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/item_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

```

```kotlin
java/com/example/affirmations/adapter/ItemAdapter.kt

    class ItemAdapter(private val context: Context,private val dataset: List<String>) : RecyclerView.Adapter<ItemAdapter.ItemViewHolder>() {
        class ItemViewHolder(private val view: View) : RecyclerView.ViewHolder(view) {
            val textView: TextView = view.findViewById(R.id.item_title)
        }

        /**
        * Create new views (invoked by the layout manager)
        */
        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ItemViewHolder {
            // create a new view item by list_item.xml
            val adapterLayout = LayoutInflater.from(parent.context).inflate(R.layout.list_item, parent, false)
            return ItemViewHolder(adapterLayout)
        }

        /**
        * Replace the contents of a view (invoked by the layout manager)
        */
        override fun onBindViewHolder(holder: ItemViewHolder, position: Int) {
            val item = dataset[position]
            holder.textView.text = item
        }

        /**
        * Return the size of your dataset (invoked by the layout manager)
        */
        override fun getItemCount() = dataset.size
    }
```
```kotlin
java/com/example/affirmations/MainActivity.kt

    class MainActivity : AppCompatActivity() {

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // Initialize data.
            val myDataset = listOf<String>('a'.toString(), 'b'.toString(), 'c'.toString(), 'd'.toString())

            val recyclerView = findViewById<RecyclerView>(R.id.recycler_view)
            recyclerView.adapter = ItemAdapter(this, myDataset)

            // Use this setting to improve performance if you know that changes
            // in content do not change the layout size of the RecyclerView
            recyclerView.setHasFixedSize(true)
        }
    }
```
