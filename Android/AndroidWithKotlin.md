*Variables*
`val abc: Int = 6;`

- Val cannot be reassigned


```kotlin
    fun main() {
        var abc: Int = 6;
        abc = 7;
        println("Hello, $abc")
        println("Hello, ${abc-1}")
    }
```
*Function return type*
```kotlin
    fun birthdayGreeting(): Unit {} 
    //when return type is Unit it means return nothing(like void)
```

```kotlin
    fun main() {
        println(birthdayGreeting("Rover"))
    }

    fun birthdayGreeting(name: String): String {
        val nameGreeting = "Happy Birthday, $name!"
        val ageGreeting = "You are now 5 years old!"
        return "$nameGreeting\n$ageGreeting"
}
```

Unlike in some languages, such as Java, where a function can change the value passed into a parameter, parameters in Kotlin are immutable. You cannot reassign the value of a parameter from within the function body.

- birthdayGreeting(name = "Rex", age = 2) // Named arguments

Android Studio with Kotlin
--------------------------

create a empty composable app
-----------------------------
- onCreate is the entry point(like main in kotlin/c/c++)
- setContent help to set app layout
- Composable app can call Composable functions (start with @Composable)
- Composable functions has no return values. only showup some values

*printng a text* 
- Text(text = "Hello $name!")

- Add background(Alt+Enter ->Surround with widget->surrount with container ex: Box,Surface)

- Add padding Text(text = "Hi, my name is $name!", modifier = Modifier.padding(24.dp))

Classes and Object in Kotlin
----------------------------

```kotlin
    fun main() {
        val myFirstDice = Dice(6)
        println("Your ${myFirstDice.numSides} sided dice rolled ${myFirstDice.roll()}!")
        
        val mySecondDice = Dice(20)
        println("Your ${mySecondDice.numSides} sided dice rolled ${mySecondDice.roll()}!")
    }

    class Dice (val numSides: Int) {

        fun roll(): Int {
            return (1..numSides).random()
        }
    }
```

Button Click
------------

```kotlin
    val rollButton: Button = findViewById(R.id.button)

    // "button" is id of button, check on tree view
    rollButton.setOnClickListener {
        val toast = Toast.makeText(this, "Some Text", Toast.LENGTH_SHORT)
        toast.show();
                
        val resultTextView: TextView = findViewById(R.id.textView)
        // textView is id of textView, check on tree view
        resultTextView.text = "6"
        
        rollDice()
        
        private fun rollDice() {
            val dice = Dice(6)
            val diceRoll = dice.roll()
            val resultTextView: TextView = findViewById(R.id.textView);
            resultTextView.text = diceRoll.toString();
        }
    }

    class Dice(private val numSides: Int) {
        fun roll(): Int {
            return (1..numSides).random();
        }
    }
```

when statement
--------------

```kotlin
    fun main() {
        val myFirstDice = Dice(6)
        val rollResult = myFirstDice.roll()
        val luckyNumber = 4

        when (rollResult) {
            luckyNumber -> println("You won!")
            1 -> println("So sorry! You rolled a 1. Try again!")
            2 -> println("Sadly, you rolled a 2. Try again!")
            3 -> println("Unfortunately, you rolled a 3. Try again!")
            5 -> println("Don't cry! You rolled a 5. Try again!")
            6 -> println("Apologies! You rolled a 6. Try again!")
        }
}
```

- findViewByID is not better way to get elements when UI is more complex,
so view binding can be used

- need to enable it from app's `build.gradle file ( Gradle Scripts > build.gradle (Module: Tip_Time.app) )`

```kotlin
    buildFeatures {
        viewBinding = true
    }
```

- click sync now

*mainActivity class replace with*

```kotlin
    class MainActivity : AppCompatActivity() {

        lateinit var binding: ActivityMainBinding

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            binding = ActivityMainBinding.inflate(layoutInflater)
            setContentView(binding.root)
        }
    }

    val myButton: Button = findViewById(R.id.my_button)
    myButton.text = "A button"

    // Better way with view binding
    val myButton: Button = binding.myButton
    myButton.text = "A button"

    // Best way with view binding and no extra variable
    binding.myButton.text = "A button"
```

*Note: The name of the binding class is generated by converting the name of the XML file to Pascal case and adding the word "Binding" to the end. Similarly, the reference for each view is generated by removing underscores and converting the view name to camel case. For example, in Pascal case activity_main.xml becomes ActivityMainBinding, and you can access @id/text_view as binding.textView.*

List and scrollView
-------------------

- Read-only list: List cannot be modified after you create it.
**type: listOf()

```kotlin
fun main() {
    //val numbers: List<Int> = listOf(1, 2, 3, 4, 5, 6)
    val numbers = listOf(1, 2, 3, 4, 5, 6)
    println("List: $numbers")
    println("Size: ${numbers.size}")

    // Access elements of the list
    println("First element: ${numbers[0]}")
    println("Second element: ${numbers[1]}")
    println("Last index: ${numbers.size - 1}")
    println("Last element: ${numbers[numbers.size - 1]}")
    println("First: ${numbers.first()}")
    println("Last: ${numbers.last()}")

    // Use the contains() method
    println("Contains 4? ${numbers.contains(4)}")
    println("Contains 7? ${numbers.contains(7)}")
    
    println("Reversed list: ${colors.reversed()}")
    println("List: $colors")
    println("Sorted list: ${colors.sorted()}")
}
```
- Mutable list: MutableList can be modified after you create it, meaning you can add, remove, or update its elements.
**type: mutableListof()

```
    val entrees = mutableListOf<String>()
    //or
    val entrees: MutableList<String> = mutableListOf()
```

- add one element - `entrees.add("noodles")`
- add more elements -  `val moreItems = listOf("ravioli", "lasagna", "fettuccine")`
			           `entrees.addAll(moreItems)`			
- remove element - `entrees.remove("spaghetti")` / `entrees.removeAt(0)` //0 is index
- clear list - `entrees.clear()`
- is empty check - `entrees.isEmpty()`

Loops
-----
while
-----
```kotlin
    val guestsPerFamily = listOf(2, 4, 1, 3)
    var totalGuests = 0
    var index = 0
    while (index < guestsPerFamily.size) {
        totalGuests += guestsPerFamily[index]
        index++
    }
    println("Total Guest Count: $totalGuests")
```
For
---
- for loop is easy and faster

```kotlin
    val names = listOf("Jessica", "Henry", "Alicia", "Jose")
    for (name in names) {
        println("$name - Number of characters: ${name.length}")
    }

    for (item in list) print(item) // Iterate over items in a list

    for (item in 'b'..'g') print(item) // Range of characters in an alphabet

    for (item in 1..5) print(item) // Range of numbers

    for (item in 5 downTo 1) print(item) // Going backward

    for (item in 3..6 step 2) print(item) // Prints: 35
```


**access resources**

- in app/src/main/res/drawable
`R.drawable.image1`

- in app/src/main/res/values/string.xml
`context.resources.getString(stringResourceId)`

- in app/src/main/res/layout/list_item.xml
`R.layout.list_item`


**Collections**

sets
----
 It's a group of related items, but unlike a list, there can't be any duplicates, and the order doesn't matter.

```kotlin
    fun main() {
        val numbers = listOf(0, 3, 8, 4, 0, 5, 5, 8, 9, 2)
        println("list:   ${numbers}")

        val setOfNumbers = numbers.toSet() //list to set convert
        println("set:    ${setOfNumbers}")

        val set1 = setOf(1,2,3)
        val set2 = mutableSetOf(3,2,1)

        println("$set1 == $set2: ${set1 == set2}") //[1, 2, 3] == [3, 2, 1]: true
        println("contains 7: ${setOfNumbers.contains(7)}") //contains 7: false
    }
```
maps
----

```kotlin
    fun main() {
        val peopleAges = mutableMapOf<String, Int>(
            "Fred" to 30,
            "Ann" to 23
        )
        println(peopleAges) //{Fred=30, Ann=23}
        peopleAges.put("Barbara", 42)
        peopleAges["Joe"] = 51
        println(peopleAges) //{Fred=30, Ann=23, Barbara=42, Joe=51}
        peopleAges["Fred"] = 31 //{Fred=31, Ann=23, Barbara=42, Joe=51}
    }
```

**forEach**

```
peopleAges.forEach { print("${it.key} is ${it.value}, ") } 
//Fred is 31, Ann is 23, Barbara is 42, Joe is 51, //has extra comma
```
**map**

```
println(peopleAges.map { "${it.key} is ${it.value}" }.joinToString(", ") )
//Fred is 31, Ann is 23, Barbara is 42, Joe is 51 //no extra comma
```
**filter**
```
val filteredNames = peopleAges.filter { it.key.length < 4 }
println(filteredNames) //{Ann=23, Joe=51}
```


*When an activity starts from scratch, you see all three of these lifecycle callbacks called in order:*

- `onCreate()` to create the app.
- `onStart()` to start it and make it visible on the screen.
- `onResume()` to give the activity focus and make it ready for the user to interact with it.

LifeCycle
---------

logging in android studio
- when override parent class lifecycle method, should be imediatly called its supper class method  `super.onStart()`

```kotlin
    override fun onStart() {
        super.onStart()
        Log.d(TAG, "onStart Called")
    }
```
```
//starting the application
    11:44:24.250  D  onCreate Called
    11:44:24.298  D  onStart Called
    11:44:24.300  D  onResume Called

//unvisible (ex : click home button and come back to screen by recent screens)
    11:44:49.038  D  onPause Called
    11:44:49.076  D  onStop Called
    11:45:04.923  D  onRestart Called
    11:45:04.924  D  onStart Called
    11:45:04.924  D  onResume Called

//partially visible (ex : share button click)
    11:46:04.393  D  onPause Called
    11:46:12.570  D  onResume Called
```

**Configuration changes**
A configuration change happens when the state of the device changes so radically that the easiest way for the system to resolve the change is to destroy and rebuild the activity.

The most common example of a configuration change is when the user rotates the device from portrait to landscape mode, or from landscape to portrait mode. A configuration change can also occur when the device language changes or a hardware keyboard is plugged in.

When a configuration change occurs, Android invokes all the activity lifecycle's shutdown callbacks. Then Android restarts the activity from scratch, running all the lifecycle startup callbacks.

When Android shuts down an app because of a configuration change, it restarts the activity with the state bundle that is available to onCreate().

As with process shutdown, save your app's state to the bundle in onSaveInstanceState().

```
    D/MainActivity: onCreate Called
    D/MainActivity: onStart Called
    D/MainActivity: onResume Called
    D/MainActivity: onPause Called
    D/MainActivity: onStop Called
    D/MainActivity: onDestroy Called
    D/MainActivity: onCreate Called
    D/MainActivity: onStart Called
```
solution --> onSaveInstanceState() // this calls after onStop()

- Saving the data each time ensures that updated data in the bundle is available to restore, if it is needed.

```kotlin
    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        Log.d(TAG, "onSaveInstanceState Called")
    }
```

```
//click on home button
    D/MainActivity: onPause Called
    D/MainActivity: onStop Called
    D/MainActivity: onSaveInstanceState Called
```

**saving data to a bundle and retrive them**

```kotlin
//KEY_REVENUE and KEY_DESSERT_SOLD are constants defined on the top of the MainActivity
    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        outState.putInt(KEY_REVENUE, revenue)
        outState.putInt(KEY_DESSERT_SOLD, dessertsSold)
        Log.d(TAG, "onSaveInstanceState Called")
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        //add below to retrive from saved bundle
        if (savedInstanceState != null) {
            revenue = savedInstanceState.getInt(KEY_REVENUE, 0)
            dessertsSold = savedInstanceState.getInt(KEY_DESSERT_SOLD, 0)
        }
    }

```
Notice that onCreate() gets a Bundle each time it is called. When your activity is restarted due to a process shut down, the bundle that you saved is passed to onCreate(). If your activity was starting fresh, this Bundle in onCreate() is null. So if the bundle is not null, you know you're "re-creating" the activity from a previously known point.