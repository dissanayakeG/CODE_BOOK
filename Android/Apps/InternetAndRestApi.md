Loading data from JsonPlaceholder
---------------------------------

*add this before `<application>` in `AndroidManifest.xml`*

```xml
    <uses-permission android:name="android.permission.INTERNET" />
```

*add these lines in `build.gradle dependencies {}`*

```kotlin
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation 'com.squareup.retrofit2:converter-gson:2.5.0'
```
    
we need recycler view as previous samples and load data from rest apis

**required addional classes**

- ServiceGenerator Object
- ApiService Iterface

**NOTE** : Be carefull about the imports
- example `import retrofit2.Callback` in `MainActivity.kt`

```xml
//layout/activity_main.xml

    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/my_recycler_view"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            tools:listitem="@layout/an_item"
            android:scrollbars="vertical"
            app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"/>

    </FrameLayout>
```

```xml
//layout/an_item.xml

    <?xml version="1.0" encoding="utf-8"?>
    <com.google.android.material.card.MaterialCardView xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
    >

        <androidx.constraintlayout.widget.ConstraintLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <TextView
                android:id="@+id/an_item_text_view"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toTopOf="parent" />

        </androidx.constraintlayout.widget.ConstraintLayout>
    </com.google.android.material.card.MaterialCardView>
```

```kotlin
//com/example/retrofit/ServiceGenerator.kt

    import okhttp3.OkHttpClient
    import retrofit2.Retrofit
    import retrofit2.converter.gson.GsonConverterFactory

    object ServiceGenerator {
        private val client = OkHttpClient.Builder().build()

        private val retrofit = Retrofit.Builder()
            .baseUrl("https://jsonplaceholder.typicode.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .client(client)
            .build()

        fun <T> buildService(service: Class<T>): T {
            return retrofit.create(service)
        }
    }
```

```kotlin
//com/example/retrofit/ApiService.kt

    import retrofit2.Call
    import retrofit2.http.GET

    interface ApiService {
        @GET("/posts")
        fun getPosts(): Call<MutableList<DataItem>>
    }
```

```kotlin
//com/example/retrofit/DataItem.kt

    data class DataItem(
        val title: String?=null,
    )
```

```kotlin
//com/example/retrofit/ItemAdapter.kt

    import android.content.Context
    import android.view.LayoutInflater
    import android.view.View
    import android.view.ViewGroup
    import android.widget.TextView
    import androidx.recyclerview.widget.RecyclerView

    class ItemAdapter( val context : Context ,private val itemList: List<DataItem>) : RecyclerView.Adapter<ItemAdapter.ViewHolder>() {

        class ViewHolder(private val view: View) : RecyclerView.ViewHolder(view) {
            var itemText : TextView = view.findViewById(R.id.an_item_text_view)
        }

        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
            val layout = LayoutInflater.from(parent.context).inflate(R.layout.an_item, parent, false)
            return ViewHolder(layout)
        }

        override fun onBindViewHolder(holder: ViewHolder, position: Int) {
            val item = itemList[position]
            holder.itemText.text = item.title
        }

        override fun getItemCount(): Int {
            return itemList.size
        }
    }
```

```kotlin
//com/example/retrofit/MainActivity.kt

    import androidx.appcompat.app.AppCompatActivity
    import android.os.Bundle
    import android.util.Log
    import androidx.recyclerview.widget.LinearLayoutManager
    import androidx.recyclerview.widget.RecyclerView
    import retrofit2.Call
    import retrofit2.Response
    import retrofit2.Callback

    class MainActivity : AppCompatActivity() {
        private lateinit var dataList: List<DataItem>
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val recyclerView: RecyclerView = findViewById(R.id.my_recycler_view)
            recyclerView.setHasFixedSize(true)

            val serviceGenerator = ServiceGenerator.buildService(ApiService::class.java)
            val call = serviceGenerator.getPosts()

            call.enqueue(object : Callback<MutableList<DataItem>> {
                //to implement below methods, Alt+Enter by clicking on object

                override fun onResponse(call: Call<MutableList<DataItem>>,response: Response<MutableList<DataItem>>) {
                    if(response.isSuccessful){
                        Log.d("Success!", response.body().toString())

                        recyclerView.apply {
                            layoutManager = LinearLayoutManager(this@MainActivity)
                            adapter = ItemAdapter(this@MainActivity, response.body()!!)
                        }
                    }
                }

                override fun onFailure(call: Call<MutableList<DataItem>>, t: Throwable) {
                    Log.e("Failed", t.message.toString())
                }

            })
        }
    }
```