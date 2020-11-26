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
    + [Creating a class:](#creating-a-class)


#### Variable Declaration

```kotlin
var height = 180
var height : Number = 180
val name : String = "peter"
```
`var` means that the variable `height` can be reassigned. Kotlin can infer the type, or you can explicitly write the type.

`val` means that the variable `name` cannot be reassigned same as `final` in Java.

You can also null safety in Kotlin:

```
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
 
