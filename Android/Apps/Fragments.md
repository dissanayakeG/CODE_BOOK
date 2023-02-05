## Simple Fragment view(Dynamically)

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

            //same as supportFragmentManager.beginTransaction().replace(R.id.fragmentView, firstFragment).commit()

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
             />

        <Button
            android:id="@+id/fragment_2_btn"
             />

        <FrameLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:id="@+id/fragmentView"
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
            android:text="Fragment 1"
             />

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
            android:text="Fragment 2"
            />

    </androidx.constraintlayout.widget.ConstraintLayout>
```

## Fragment Communication

**Note** to communicate among fragments we need to implement an interface in main activity

```kotlin
//com/example/fragmentcommunicator/MainActivity.kt

    class MainActivity : AppCompatActivity(), Communicator {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val firstFragment = FirstFragment()

            supportFragmentManager.beginTransaction().replace(R.id.fragment_container, firstFragment)
                .commit()
        }

        override fun dataPass(parsString: String) {
            val bundle = Bundle()
            bundle.putString("message", parsString)

            val transaction = supportFragmentManager.beginTransaction()
            val secondFragment = SecondFragment()

            secondFragment.arguments = bundle

            transaction.replace(R.id.fragment_container, secondFragment)
            transaction.commit()
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

        <FrameLayout            
            android:id="@+id/fragment_container"/>

    </androidx.constraintlayout.widget.ConstraintLayout>
```

```kotlin
//com/example/fragmentcommunicator/fragments/Communicator.kt
    interface Communicator {
        fun dataPass(parsString: String)
    }
```

```kotlin
//com/example/fragmentcommunicator/fragments/FirstFragment.kt

    class FirstFragment : Fragment() {

        lateinit var communicator: Communicator

        override fun onCreateView(
            inflater: LayoutInflater, container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? {
            val view = inflater.inflate(R.layout.fragment_first, container, false)

            communicator = activity as Communicator

            val sendBtn = view.findViewById<Button>(R.id.fragment_1_send_button)
            val inputMessage = view.findViewById<EditText>(R.id.fragment_1_edit_text)

            sendBtn.setOnClickListener {
                communicator.dataPass(inputMessage.text.toString())
            }

            return view
        }
    }
```

```xml
//layout/fragment_first.xml

    <?xml version="1.0" encoding="utf-8"?>
    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".fragments.FirstFragment">

        <EditText
            android:id="@+id/fragment_1_edit_text"
           />

        <Button
            android:id="@+id/fragment_1_send_button"
            />

    </androidx.constraintlayout.widget.ConstraintLayout>
```

```kotlin
//com/example/fragmentcommunicator/fragments/SecondFragment.kt

    class SecondFragment : Fragment() {

        var displayString:String? =""

        override fun onCreateView(
            inflater: LayoutInflater, container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? {
            val view = inflater.inflate(R.layout.fragment_second, container, false)

            displayString = arguments?.getString("message")
            val textView = view.findViewById<TextView>(R.id.fragment_2_text_view)

            textView.text = displayString

            return view
        }
    }
```

```xml
//layout/fragment_second.xml

    <?xml version="1.0" encoding="utf-8"?>
    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".fragments.FirstFragment">

        <TextView
            android:id="@+id/fragment_2_text_view"
            />

    </androidx.constraintlayout.widget.ConstraintLayout>
```
