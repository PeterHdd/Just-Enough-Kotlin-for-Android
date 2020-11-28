### Just Enough Kotlin For Android

There are many tutorials about Kotlin online, this repository is just meant to be as a reference when using Kotlin in Android.

Kotlin is a cross-platform, statically typed, general-purpose programming language with type inference, and it is designed to work with Java.

Let's get started!

### Table of Content

- [Just Enough Kotlin For Android](#just-enough-kotlin-for-android)
- [Table of Content](#table-of-content)
  * [Variable Declaration](#variable-declaration)
  * [when if loops](#when-if-loops)
  * [Classes](#classes)
    + [Creating a class:](#creating-a-class-)
  * [Secondary Constructor](#secondary-constructor)
  * [Initializing Properties](#initializing-properties)
  * [Getter and Setters](#getter-and-setters)
  * [Type of Classes](#type-of-classes)


#### Variable Declaration

```kotlin
var height = 180
var height : Number = 180
val name : String = "peter"
```
`var` means that the variable `height` can be reassigned. Kotlin can infer the type, or you can explicitly write the type.

`val` means that the variable `name` cannot be reassigned same as `final` in Java.

You can also null safety in Kotlin:

```kotlin
fun main(){
    var name : String? = null
    val age : Number? = null
    print(name?.length)
    print(age!!.toShort())
}
```
In Kotlin all variables are null safe, meaning you cannot assign them to null unless you create a nullable variable.

In the above code, both `name` and `age` are nullable since we used `?`. Now since `name` is nullable, everytime we call a property or method on the variable `name` then we have to either check if it is null or use safe calls as I did in the example.
So, in the above example, the `print()` statement would return `null` since `name` is null.

Regarding the variable `age` , I used the not-null assertion operator which would throw a `KotlinNullPointerException`.
You can also use the elvis operator when doing conditions, for example:

```kotlin
val result = name?.endsWith("r") ?: "peter"
print(result)
```
The above will check if `name?.endsWith("r")` is not null and return the result, else if it is null it will then return `peter`.

#### when if loops

```kotlin
when(age) {
    10 -> print("I'm $age years old")
    5  -> print("I'm $age years old")
    else -> print("invalid number")
 }
```
In Kotlin, the `when` expression is the same as the `switch` in Java. Here, it will check every case until one of them is satisfied.
You can also move variable declaration inside the `when`:

```kotlin
when(val age = 5) {
    10 -> print("I'm $age years old")
    5  -> print("I'm $age years old")
    else -> print("invalid number")
}
```
In Kotlin, you can use the `if` conditions as expression, meaning you can assign the result to a variable:

```kotlin
var age = 5
  val result  = if(age == 5) {
      age += age
      age
  }else {
      age
  }
```
The for loop in Kotlin is different than Java, in which you have to do the following:

```kotlin
for (i in 1..4){
    print(i)
}
```
So here, it will iterate from 1 till 4, and if you dont want to include the last number then you can use `until` keyword.

To iterate inside a collection, you can do the following:

```kotlin
val listOfAnimals : List<String> = mutableListOf("wolf","dogs","cats")
listOfAnimals.forEach {
    println(it)
}

for(animal in listOfAnimals){
    println(animal)
}
```
So here, `mutableListof()` is a top level function which means it does not belong to any class. The `mutableListof()` will create a list in which we can add or remove elements.
While the `listOf()` will create an immutable list (can't change).

The `forEach()` is a function, which takes another function as an argument and iterates through each element, also the `for in` will iterate each element.

#### Classes
In Kotlin, we have four visibility modifiers: public (default), private, protected, internal. The only modifier that is different than Java is `internal`, which basically means that the variable, class.. is visible inside the same module.

##### Creating a class:

```kotlin
class Example {
     var name = "peter"
     var location = "Lebanon"

    constructor(name : String, location: String){
        this.name = name
        this.location = location
    }
}

fun main(){
    var example = Example("john","England")
    print(example.name)
}
```
As, you can see in Kotlin you have properties, and these properties will have implicit getter and setter. Classes created in Kotlin by default would be `final`, which means you cannot extend them unless you specify by using the `open` keyword.

In the `main` function, we create an instance of the class `Example`. Now the above code be converted to the following:

```kotlin
class Example constructor(var name: String, var location: String) {
}
```
Here you have a primary `constructor`, we can also remove the keyword `constructor` and the empty body:

```kotlin
class Example(var name: String, var location: String)
```
---

Now let's say you remove the `var` keyword from the property `name` like the following:

```kotlin
class Example(name: String, var location: String)
```
If you try to access name when creating an instance of `Example`, you will get an error since if you remove either `var` or `val` in the primary constructor it then means
that you are only passing `name` as an argument to constructor, but it is not a property in class `Example`. The decompile java class would be:

```Java
public final class Example {
    @NotNull
    private String location;

    @NotNull
    public final String getLocation() {
        return this.location;
    }

    public final void setLocation(@NotNull String var1) {
        this.location = var1;
    }

    public Example(@NotNull String name, @NotNull String location) {
        super();
        this.location = location;
    }
}
```
As you can see `name` is just passed to the `Example` constructor. 

If you have a primary constructor in your class, then you can use `init` block to do some initialization. For example:

```kotlin
class Example(val name: String , val location: String){
    init {
        print("initialize")
    }
}
```
The `init` block will run immediately after the primary constructor, if you decompile it to Java then you will see that the `init` block code will be inside the constructor.

##### Secondary Constructor

You can also have more than one constructor, but the second constructor must refer to the primary constructor:

```kotlin
class Example(val name: String){
    init {
        print("initialize")
    }
    constructor(name: String, location: String) : this(name){
        print("second constructor")
    }
}
```
If you decompile the above code you will see that in Java you would also have 2 constructors thus having overloaded constructor. Also the `init` block
will still be called immediately after the primary constructor.

#### Initializing Properties

Sometimes in your code, you don't want to immediately initialize a variable and since Kotlin is null safe, then you cannot do this `var list : List<String>` as a declaration.

The above code will throw an error: `Property must be initialized or be abstract`. Therefore, to solve this you can use the keyword `lateinit` which can only work with variables that can be changed (`var`). The `lateinit`, in simple terms
basically means initialize later:

```kotlin
class Example(val name: String){
       lateinit var list : List<String>
        constructor(name : String, location : String) : this(name){
            if(::list.isInitialized){
                print("initialized")
            }
        }
}
```
As you can see `list` here is using `lateinit`, and we use the method `isInitialized` to check if the variable has been initialized or not. If it's not initialized then we
will get an error `UninitializedPropertyAccessException`. The `::` in Kotlin is used for reflection, using reflection you can get information about a class, function or property.

----

Another way to initialize properties is by using the `by lazy {}`. The `by lazy{}` uses delegation properties, which means that you pass the responsibility to another class or method. For example:

```kotlin
class Example(val name: String){
        private val list : List<String> by lazy { mutableListOf("example1") }
        constructor(name : String, location : String) : this(name){
            print(list[0])
        }
}
```
To use property delegation, you need to declare the property and then use the keyword `by <expression>`.

In this case the `lazy()` function will return an instance of [`Lazy`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-lazy/) which is a delegate, therefore now the `Lazy` instance is responsible for the property `list`. So, every time you access the getter function of this property, it will be delegated to the [`getValue()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/get-value.html) method inside `Lazy`.

When you use `by lazy`, the variable will not be initialized until you first used, and then it will always return the same value, for example:

```kotlin
class Example(val name: String){
        private val list : List<String> by lazy {
            println("lazy initial")
            mutableListOf("example1")
        }
        constructor(name : String, location : String) : this(name){
            println("first call ${list[0]}")
            println("second call ${list[0]}")
        }
}

fun main(){
    var example = Example("peter","LB")
}
```
The above code will output:

```kotlin
lazy initial
first call example1
second call example1
```
So, as you can see when we used the variable it initialized it and then later on, the same value kept returning (initialized only once).

You can see the difference between `lateinit` and `by lazy` [here](https://stackoverflow.com/questions/36623177/kotlin-property-initialization-using-by-lazy-vs-lateinit).

In Android, property delegation is used alot, for example the [`viewModels()`](https://developer.android.com/reference/kotlin/androidx/fragment/app/package-summary#viewmodels), which returns a value of type `Lazy`, therefore the `viewModels()` is the same as `by lazy`:

```kotlin
// creates lazy delegate for obtaining zero-argument MyViewModel
private val viewModel : MyViewModel by viewModels()
// it's functionally equal to:
private val viewModel by lazy {
    ViewModelProvider(this).get(MyViewModel::class.java)
}
```
Another example is the `observeAsState()` function which returns an instance of `State<T>`, and since [`State`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/State.html) contains the method `getValue()`, then it can be used for property delegation.

#### Getter and Setters

Kotlin auto generates the getters and setters for us, so if we have:

```kotlin
class Example{
    var name : String = "peter"
}
```
Then in Java it will be:

```java
public final class Example {
    @NotNull
    private String name = "peter";

    @NotNull
    public final String getName() {
        return this.name;
    }

    public final void setName(@NotNull String var1) {
        this.name = var1;
    }
}
```
As, you can see the field is private and we access it using the getter and setter that we always used in Java.

Now, let's say you want custom getter and setter, then you can do the following:

```kotlin
class Example{
    val name : String
    get() {
        print("name")
        return "test"
    }
}
```
In Java, the above code would be:

```Java
public final class Example {
    @NotNull
    public final String getName() {
        String var1 = "name";
        return "test";
    }
}
```
As, you can see in this example, the `name` property in Kotlin is not a field in Java, while in the first example of this section the `name` property without a custom getter, was a field in Java.
In Kotlin, there is a concept called backing field, if a property has a backing field, then it will store its value in memory. For example:

```kotlin
class Example{
    val location : String
    get() = "location"
    
    var name : String = ""
        get() {
        print("name")
        return "test"
    }
    set(value) { field = value }
}
```
In the code above `name` property has a backing field (`field` keyword), therefore in Java we will get the following:

```Java
public final class Example {
    @NotNull
    private String name = "";

    @NotNull
    public final String getLocation() {
        return "location";
    }

    @NotNull
    public final String getName() {
        String var1 = "name";
        System.out.print(var1);
        return "test";
    }

    public final void setName(@NotNull String value) {
        this.name = value;
    }
}
```
#### Type of Classes

There are different type of classes in Kotlin:

1- Data Classes: are classes that are mainly used to hold data, example:

```kotlin
data class Example(val name : String)
```
The java equivalent will contain the getter/setter for the name field, `hashCode()`,`equal()`, `toString()` methods.

2- Sealed Classes: are classes used when you want to fix type hierarchies and when you want a value to only have the type declared, for example:

```kotlin
sealed class People
class Student : People()
class Teacher : People()

fun feed(ppl: People): String {
    return when(ppl) {
        is Student -> "I'am a Student"
        is Teacher -> "I'am a Teacher"
    }
}


fun main(){
    print(feed(Student()))
}
```

3- Open classes: are classes that you can extend, since by default all classes are final, for example:

```kotlin
open class People{
    open fun getName(){
        println("I'm the parent class ${People::class.simpleName}")
    }
}
class Student : People(){
    override fun getName() {
         super.getName()
        println("I'm the parent class ${Student::class.simpleName}")
    }
}
fun main(){
    var student = Student()
    println(student.getName())
}
```
This will give the following output:

```kotlin
I'm the parent class People
I'm the parent class Student
```
To extend the class you have to use the following `<child-class> : <parent-class>()`.

If you have an interface and you want to implement it, then you have to do `<class> : <interface>`.