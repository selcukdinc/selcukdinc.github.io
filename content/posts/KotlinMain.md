+++
title = 'KotlinMain'
date = 2024-10-04T22:39:36+03:00
draft = false
ShowToc = true
+++
[Video Source](https://youtu.be/F9UC9DY-vIU?si=C_nlP7wMe2GGxkJs)
## Variables
```
// Changable variables
var [varaible name]: [variable type] = [value of variable]
var [varaible name]: [variable type]? = null

// consts
val [name]: [Type] = [value]
// vals doesnt changing in future
```
## Workflows
```
// out of main function
val name: String = "Selcuk"
var greeting: String? = null
```
#### If Control Block 
```
// inside of main func
// greeting = "Alright, Welcome!!!"
if (greeting != null) {
        println(greeting)
    } else {
        println("Hi")
    }
    println(name)
```
#### When Control Block
```
// inside of main func
// greeting = "Alright, Welcome!!!"
    when(greeting){
        null -> println("Hi")
        else -> println(greeting)
    }
    println(name)
```
#### Like a Bitwise
```
val greetingToPrint = if(greeting != null) greeting else "Hi"
print(greetingToPrint)
println(name)
``` 
#### Combine When and Val things
```
val greetingToPrint = when(greeting){
        null -> "Hi"
        else -> greeting
    }
print(greetingToPrint)
println(name)
```

## Basic Functions
```
fun [function Name}(): [Function Return Type}? {
    return null
}

fun getGreeting(): String? {
    return "Hello Everybody!"
}
// or 
fun getGreeting() = "Hello Kotlin"


// Non return function type
fun sayHello(): Unit {
    getGreeting()
}
// or
fun sayHello() {
    println(getGreeting())
}


// usage (like inside of main)
    println(getGreeting())
    sayHello()

// Casting and use parameters
fun sayHello(greeting:String, itemToGreet:String) = println("$greeting $itemToGreet")
```
## Collections & Iteration
#### Arrays
```
    val interestingThings = arrayOf("Kotlin", "Programming", "Comic Books")
    
    println(interestingThings.size) // output : 3
    println(interestingThings[0]) // output : Kotlin
    println(interestingThings.get(0)) // output : Kotlin

    
    for (interestingThing in interestingThings){
        println(interestingThing)
    }

    interestingThings.forEach {interestingThing ->
        println(interestingThing)
    }

    // output : Kotlin
    // output : Programming
    // output : Comic books


    interestingThings.forEachIndexed { index, interestingThing ->
        println("$interestingThing is at index $index")
    }
    // output : Kotlin is at index 0 
    // output : Programming is at index 1
    // output : Comic books is at index 2
```
#### Lists
```
    // immutable lists, doesnt accept new value
    val interestingThings = listOf("Kotlin", "Programming", "Comic Books")

    // mutable lists, accept new values
    val interestingThings = mutableListOf("Kotlin", "Programming", "Comic Books")
    interestingThings.add("Dogs")

    interestingThings.forEach { interestingThing ->
        println(interestingThing)
    }
```
#### Maps
```
    // immutable maps : doesnt accept new values
    val map = mapOf(1 to "a", 2 to "b", 3 to "c")
    
    // mutable maps : accept new values
    val map = mutableMapOf(1 to "a", 2 to "b", 3 to "c")
    map.put(4, "d")

    map.forEach { key, value -> println("$key -> $value") }
```

### Combine List and Functions
```
fun sayHello(greeting:String, itemToGreet:List<String>) {
    itemToGreet.forEach { itemToGreet ->
        println("$greeting $itemToGreet")
    }
}

fun main() {
    val interestingThings = mutableListOf("Kotlin", "Programming", "Comic Books")
    sayHello("Hi", interestingThings)

}
// output : Hi Kotlin
// output : Hi Programming
// output : Hi Comic Books

```
## Vararg, named arguments & default parameter values
#### vararg

```
fun sayHello(greeting:String, vararg itemsToGreet:String) {
    itemsToGreet.forEach { itemToGreet ->
        println("$greeting $itemToGreet")
    }
}

fun main() {
    val interestingThings = mutableListOf("Kotlin", "Programming", "Comic Books")
    sayHello("Hi")
}
```
#### Type Dismatch
If we change mutablelistOf -to> arrayOf
```
fun main(){
    val interestingThings = arrayOf("Kotlin", "Programming", "Comic Books")
    sayHello("Hi", *interestingThings) // add asterisc
}
```
#### Default Parameters
```
fun greetPerson(greeting: String = "Hello", name: String = "Kotlin") = 
                println("$greeting $name")

fun main() {
    greetPerson(name = "Selcuk", greeting = "hi") // output : hi Selcuk
    greetPerson(name = "Selcuk") // output : Hello Selcuk
    greetPerson() // output : Hello Kotlin
    
}
```
#### Final Look
```
fun sayHello(greeting:String, vararg itemsToGreet:String) {
    itemsToGreet.forEach { itemToGreet ->
        println("$greeting $itemToGreet")
    }
}
fun main() {
    val interestingThings = arrayOf("Kotlin", "Programming", "Comic Books")
    sayHello(itemsToGreet = interestingThings, greeting = "Hi")
}
```
## Classes
Create new class
```
class Person(_firstName: String, _lastName: String){
    val firstName: String
    val lastName: String
}
```
usage in main
```
val person = Person("Peter", "Parker");
    person.lastName
    person.firstName
```
#### Initialze Variables
##### Use Init block
```
class Person(_firstName: String, _lastName: String){
    val firstName: String
    val lastName: 
    
    init {
        firstName = _firstName
        lastName = _lastName
    }
}
```
##### Use Directly
```
class Person(_firstName: String, _lastName: String){
    val firstName: String = _firstName
    val lastName: String = _lastName
}
```
#### Initialize and Constructors
```
class Person(val firstName: String = "Peter", val lastName: String = "Parker"){

    init {
        println("init 1")
    }

    constructor(): this("Peter", "Parker"){
        println("Secondary constructor")
    }

    init {
        println("init 2")
    }

    // if main params is null
    // output : init 1
    // output : init 2
    // output : Secondary constructor
    // else
    // output : init 1
    // output : init 2
}
```
#### Get & Set
```
class Person(val firstName: String = "Peter", val lastName: String = "Parker"){
    var nickName: String? = null
        set(value){
            field = value
            println("the new nick name is $value")
        }
        get(){
            println("the returned value is $field")
            return field
        }
}
```
#### PrintInfo()
``` 
    fun printInfo(){
        // Method 1
        // val nickNameToPrint = if(nickName != null) nickName else "[no nickname]"
        // Method 2
        val nickNameToPrint = nickName ?: "[no nickname]"
        println("$firstName $nickNameToPrint $lastName")
    }
```
