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
## Interfaces
Create interface
```
interface PersonInfoProvider {
    
}
```
#### Override Function
```
interface PersonInfoProvider{
    fun printInfo(person: Person)
}

class BasicInfoProvider : PersonInfoProvider {
    override fun printInfo(person: Person) {
        println("basicInfoProvider")
        person.printInfo()
    }
}

fun main() {
    val provider = BasicInfoProvider()

    provider.printInfo(Person())
}
```
##### Override val
```
interface PersonInfoProvider{
    val providerInfo: String
    fun printInfo(person: Person) {
        println(providerInfo)
        person.printInfo()
    }
}

class BasicInfoProvider : PersonInfoProvider {
    override val providerInfo: String
        get() = "BasicInfoProvider"

    override fun printInfo(person: Person) {
        super.printInfo(person)
        println("Additional print statement")
    }
}

fun main() {
    val provider = BasicInfoProvider()

    provider.printInfo(Person())
}
```
Final touch 
```
interface PersonInfoProvider{
    val providerInfo: String
    fun printInfo(person: Person) {
        println(providerInfo)
        person.printInfo()
    }
}

interface SessionInfoProvider {
    fun getSessionId(): String
}

class BasicInfoProvider : PersonInfoProvider, SessionInfoProvider {
    override val providerInfo: String
        get() = "BasicInfoProvider"

    override fun printInfo(person: Person) {
        super.printInfo(person)
        println("Additional print statement")
    }

    override fun getSessionId(): String {
        return "Session"
    }
}

fun main() {
    val provider = BasicInfoProvider()

    provider.printInfo(Person())
    provider.getSessionId()

    checkTypes(provider)
}

fun checkTypes(infoProvider: PersonInfoProvider){
    if (infoProvider !is SessionInfoProvider){
        println("not a session info provider")
    }else{
        println("is a session info provider")
        // Method 1
        //(infoProvider as SessionInfoProvider).getSessionId()
        // Method 2, Smart casting
        infoProvider.getSessionId()
    }
}
```
## Inheritance
Create new kotlin file
##### FancyInfoProvider.kt
```
class FancyInfoProvider : BasicInfoProvider() {

    override val sessionIdPrefix: String
        get() = "Fancy Session"

    override val providerInfo: String
        get() = "Fancy Info Provider"

    override fun printInfo(person: Person) {
        super.printInfo(person)
        println("Fancy Info")
    }
}
```
##### PersonInfoProvider
```
interface PersonInfoProvider{
    val providerInfo: String
    fun printInfo(person: Person) {
        println(providerInfo)
        person.printInfo()
    }
}

interface SessionInfoProvider {
    fun getSessionId(): String
}

open class BasicInfoProvider : PersonInfoProvider, SessionInfoProvider {
    override val providerInfo: String
        get() = "BasicInfoProvider"


    protected open val sessionIdPrefix = "Session"


    override fun printInfo(person: Person) {
        super.printInfo(person)
        println("Additional print statement")
    }

    override fun getSessionId(): String {
        return sessionIdPrefix
    }
}

fun main() {
    val provider = FancyInfoProvider()


    provider.printInfo(Person())
    provider.getSessionId()

    checkTypes(provider)
}

fun checkTypes(infoProvider: PersonInfoProvider){
    if (infoProvider !is SessionInfoProvider){
        println("not a session info provider")
    }else{
        println("is a session info provider")
        //(infoProvider as SessionInfoProvider).getSessionId()
        infoProvider.getSessionId()
    }
}

```

## Object Expressions
```
    val provider = object : PersonInfoProvider{
        override val providerInfo: String
            get() = "New Info Provider"

        fun getSessionId() = "id"
    }
```
## Companion Objects
```
interface IdProvider {
    fun getId(): String
}

class Entity private constructor(val id: String) {

    companion object Factory : IdProvider{
        override fun getId(): String {
            return "123"
        }

        const val id = "id"
        
        fun create() = Entity(id)
    }
}

fun main() {

    val entity = Entity.Factory.create()
    Entity.id
}
```

## Object Declaration

```
object EntityFactory {
    fun create() = Entity("id", "name")
}

class Entity (val id: String, val name: String) {
    override fun toString(): String {
        return "id:$id name: $name"
    }
}

fun main() {

    val entity = EntityFactory.create()
    println(entity)
}
```

## Enum Classes
```
import java.util.UUID

enum class EntityType {
    EASY, MEDIUM, HARD;

    fun getFormattedName() = name.toLowerCase().capitalize()
}

object EntityFactory {
    fun create(type: EntityType) : Entity {
        val id = UUID.randomUUID().toString()

        val  name = when(type){
            EntityType.EASY -> type.name
            EntityType.MEDIUM -> type.getFormattedName()
            EntityType.HARD -> "Hard"
        }

        return Entity(id, name)
    }
}

class Entity (val id: String, val name: String) {
    override fun toString(): String {
        return "id:$id name: $name"
    }
}

fun main() {

    val entity = EntityFactory.create(EntityType.EASY)
    println(entity)

    val mediumEntity = EntityFactory.create(EntityType.MEDIUM)
    println(mediumEntity)
}
```
## Sealed Classes
```
import java.util.UUID

enum class EntityType {
    HELP, EASY, MEDIUM, HARD;

    fun getFormattedName() = name.toLowerCase().capitalize()
}

object EntityFactory {
    fun create(type: EntityType) : Entity {
        val id = UUID.randomUUID().toString()

        val  name = when(type){
            EntityType.EASY -> type.name
            EntityType.MEDIUM -> type.getFormattedName()
            EntityType.HARD -> "Hard"
            EntityType.HELP -> type.getFormattedName()
        }

        return when(type){
            EntityType.EASY -> Entity.Easy(id, name)
            EntityType.MEDIUM -> Entity.Medium(id, name)
            EntityType.HARD -> Entity.Hard(id, name, 2f)
            EntityType.HELP -> Entity.help
        }
    }
}

sealed class Entity () {
    object help : Entity() {
        val name = "help"
    }
    data class Easy(val id: String, val name: String): Entity()
    data class Medium(val id: String, val name: String): Entity()
    data class Hard(val id: String, val name: String, val multiplier: Float): Entity()
}

fun main() {
    val entity:Entity = EntityFactory.create(EntityType.HARD)
    val msg = when(entity){
        Entity.help -> "help class"
        is Entity.Easy -> "easy class"
        is Entity.Medium -> "medium class"
        is Entity.Hard -> "hard class"
    }

    println(msg)
}
```

## Data Classes
```
fun main() {
    val entity1 = Entity.Easy("id", "name")
    val entity2 = Entity.Easy("id", "name")

    if(entity1 === entity1){
        println("they are equal")
    } else {
        println("they are not equal")
    }
}
```

## Extensin Functions / Properties
```
[outside of main]
fun Entity.Medium.printInfo(){
    println("Medium class: $id")
}

val Entity.Medium.info: String
    get() = "some info"

[inside of main]
   Entity.Medium(id = "id", name = "name").printInfo()

   val entity1 = Entity.Easy("id", "name")
    val entity2 = EntityFactory.create(EntityType.MEDIUM)
    if (entity2 is Entity.Medium){
        entity2.printInfo()
        entity2.info
    }
```

## Advanced Functions
Created HigerOrderFunctions.kt
```
fun printFilteredStrings(list: List<String>, predicate: ((String) -> Boolean)?){
    list.forEach {
        if(predicate?.invoke(it) == true){
            println(it)
        }
    }
}

val predicate: (String) -> Boolean = {
    it.startsWith("J")
}

fun getPrintPredicate(): (String) -> Boolean{
    return { it.startsWith("J") }
}

fun main(){
    val list = listOf("Kotlin", "Java", "C++", "Javascript")
    printFilteredStrings(list, getPrintPredicate())
    
    printFilteredStrings(list, null)
}
```
## Combine Functions
Functional.kt file created
```
    val list = listOf("Kotlin", "Java", "C++", "Javascript", null, null)
    list
        .filterNotNull()
        .filter {
            it.startsWith("J")
        }
        .map {
            it.length
        }
        .forEach {
        println(it)
    }
// output : 4
// output : 10
```

```
    list
        .filterNotNull()
        .takeLast(3) // .take => takes first 3 item, .takeLast => takes last 3 item
        .forEach {
        println(it)
    }
// output : Java
// output : C++
// output : Javascript
```

```
list
        .filterNotNull()
        .associate { it to it.length }
        .forEach {
        println("${it.value}, ${it.key}")
    }
// output : 6, Kotlin
// output : 4, Java
// output : 3, C++
// output : 10, Javascript
```
# Basic Playable Rock & Paper & Scissor Game
```
fun main(){
    var computerChoice = ""
    var playerChoice = ""
    print("Rock, Paper or Scissors? Enter your choice\n")
    print("Choice : ")
    playerChoice = readln()
    playerChoice = playerChoice.lowercase()
    
    while (playerChoice != "rock" && playerChoice != "paper" && playerChoice != "scissors"){
        print("Wrong Choice, try again : ")
        playerChoice = readln()
        playerChoice = playerChoice.lowercase()

    }
    val randomNumber = (1..3).random()
    when (randomNumber){
        1 -> computerChoice = "rock"
        2 -> computerChoice = "paper"
        3 -> computerChoice = "scissors"
    }
    println(computerChoice)
    val winner = when{
        playerChoice == computerChoice -> "Tie"
        playerChoice == "rock" && computerChoice == "scissors" -> "Player"
        playerChoice == "scissors" && computerChoice == "paper" -> "Player"
        playerChoice == "paper" && computerChoice == "rock" -> "Player"
        else -> "Computer"
    }
    when(winner){
        "Tie" -> println("is Tie!")
        else -> println("$winner win this game")
    }
}
```

# Create Coffee Machine Basics on Console
```
data class CoffeeDetails(
    val sugarCount: Int,
    val name: String,
    val size:String,
    val creamAmount: Int)

fun main(){
    val coffeeForDenis = CoffeeDetails(2, "denis", "xxl", 1)
    askCoffeeDetails()
}

fun askCoffeeDetails(){
    println("Who is this coffee for ? ")
    val name = readln()
    println("How many pieces of sugar do you want?")
    val sugarCount = readln().toIntOrNull()
    println("How many cream do you want?")
    val creamCount = readln().toIntOrNull()
    println("How much coffee do you want?")
    val coffeeSize = readln()

    // call Function
    if (sugarCount != null && creamCount != null){
        makeCoffee(CoffeeDetails(sugarCount, name, coffeeSize, creamCount))
    }
    else
        println("wrong input! Sugar or cream amount is should be int format")
}
// Define Function
fun makeCoffee(coffeeDetails: CoffeeDetails){
    when{
        coffeeDetails.sugarCount < 0 && coffeeDetails.creamAmount < 0->
            println("Coffee with doesn't accept negative sugar and cream amount Dear ${coffeeDetails.name}, preparing sugar and cream free coffee")

        coffeeDetails.sugarCount < 0 && coffeeDetails.creamAmount == 0 ->
            println("Coffee with doesn't accept negative sugar amount Dear ${coffeeDetails.name}, preparing sugar and cream free coffee")

        coffeeDetails.sugarCount == 0 && coffeeDetails.creamAmount < 0 ->
            println("Coffee with doesn't accept negative cream amount Dear ${coffeeDetails.name}, preparing sugar and cream free coffee")

        coffeeDetails.sugarCount == 0 && coffeeDetails.creamAmount == 0->
            println("Coffee is ready! with zero sugar and zero cream for${coffeeDetails.name}")

        coffeeDetails.sugarCount == 0 && coffeeDetails.creamAmount > 0->
            println("Coffee is ready! with zero sugar and added ${coffeeDetails.creamAmount} " +
                    "cream for${coffeeDetails.name}")

        coffeeDetails.sugarCount == 1 ->
            println("Coffee is ready! with ${coffeeDetails.sugarCount} " +
                    "spoon of sugar and added ${coffeeDetails.creamAmount} " +
                    "cream for ${coffeeDetails.name}")

        else ->
            println("Coffee is ready! with ${coffeeDetails.sugarCount} " +
                    "spoons of sugar and added ${coffeeDetails.creamAmount} " +
                    "cream for ${coffeeDetails.name}")
    }
}
``` 