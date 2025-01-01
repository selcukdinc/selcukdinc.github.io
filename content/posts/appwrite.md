+++
title = 'Appwrite Android (Kotlin)'
date = 2024-11-20T17:49:20+03:00
draft = false
ShowToc = true
+++
### Login Function in AppWrite
```
suspend fun login(email: String, password: String): User<Map<String, Any>>? {
        return try {
            account.createEmailPasswordSession(email, password)
            getLoggedIn()
        } catch (e: AppwriteException) {
            null
        }
    }
```
### Register Function
```
suspend fun register(email: String, password: String): User<Map<String, Any>>? {
        return try {
            account.create(ID.unique(), email, password)
            login(email, password)
        } catch (e: AppwriteException) {
            null
        }
    }
```
### Keep Login Function
```
suspend fun getLoggedIn(): User<Map<String, Any>>? {
    return try {
        account.get()
    } catch (e: AppwriteException) {
        null
    }
}
```
### Logout Function
```
suspend fun logout() {
    account.deleteSession("current")
}
```