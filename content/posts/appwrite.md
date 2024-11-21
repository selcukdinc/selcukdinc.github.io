+++
title = 'Appwrite Android (Kotlin)'
date = 2024-11-20T17:49:20+03:00
draft = false
+++
### Login Function
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