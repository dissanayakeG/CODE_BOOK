Simple Fragment view(Dynamically)
---------------------------------

```kotlin
//com/example/fragments/MainActivity.kt

    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val firstFragment = FirstFragment()
            val secondFragment = SecondFragment()

            supportFragmentManager.beginTransaction().apply {
                replace(R.id.fragmentView, firstFragment)
                commit()
            }

            val f1btn = findViewById<Button>(R.id.fragment_1_btn)
            val f2btn = findViewById<Button>(R.id.fragment_2_btn)

            f1btn.setOnClickListener {
                supportFragmentManager.beginTransaction().apply {
                    replace(R.id.fragmentView, firstFragment)
                    commit()
                }
            }

            f2btn.setOnClickListener {
                supportFragmentManager.beginTransaction().apply {
                    replace(R.id.fragmentView, secondFragment)
                    commit()
                }
            }
        }
    }
```

```xml
//layout/activity_main.xml

    <?xml version="1.0" encoding="utf-8"?>
    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <Button
            android:id="@+id/fragment_1_btn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/fragment_1"
            android:textSize="20sp"
            app:layout_constraintEnd_toStartOf="@+id/fragment_2_btn"
            app:layout_constraintHorizontal_bias="0.5"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />

        <Button
            android:id="@+id/fragment_2_btn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="48dp"
            android:text="@string/fragment_2"
            android:textSize="20sp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.5"
            app:layout_constraintStart_toEndOf="@+id/fragment_1_btn"
            app:layout_constraintTop_toTopOf="parent" />

        <FrameLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:id="@+id/fragmentView"
            app:layout_constraintTop_toBottomOf="@id/fragment_2_btn"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"
            tools:layout="@layout/fragment_first" />

            //if you want to set static fragment use fragment tag instead of FrameLayout and use name property with fragment name

    </androidx.constraintlayout.widget.ConstraintLayout>
```

```kotlin
//com/example/fragments/FirstFragment.kt

    class FirstFragment : Fragment(R.layout.fragment_first) {

    }
```

```kotlin
//com/example/fragments/SecondFragment.kt

    class SecondFragment : Fragment(R.layout.fragment_second) {

    }
```

 **Note:** inflating fragment inside onCreateView and inflating in constructor is same

```kotlin
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_blank, container, false)
    }
    //is equel to
    class SecondFragment : Fragment(R.layout.fragment_second){}
```

```xml
//layout/fragment_first.xml

    <?xml version="1.0" encoding="utf-8"?>
    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".FirstFragment">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Fragment 1"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.5"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintVertical_bias="0.5" />

    </androidx.constraintlayout.widget.ConstraintLayout>
```

```xml
//layout/fragment_second.xml

    <?xml version="1.0" encoding="utf-8"?>
    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".FirstFragment">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Fragment 2"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.5"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintVertical_bias="0.5" />

    </androidx.constraintlayout.widget.ConstraintLayout>
```
