+++
title = 'AndroidBasics'
date = 2024-10-13T11:29:49+03:00
draft = false
ShowToc = true
+++
### Create Toast With Clik the Button
```
val context = LocalContext.current
Button(onClick = {
    Toast
        .makeText(
            context,
            "Thanks for Clicking!",
            Toast.LENGTH_LONG).show()
}) {
    Text("Click Me!")
}
```

### ?:  -  Elvis Operator
if variable goes null then give it default value
```
val inputValueDouble = inputValue.toDoubleOrNull() ?: 0.0
```

### Nullable String
```
val name : String? = "Ella"
name?.let{
println(it..uppercase())
}
```

### Give Space Between UI Elements
```
// vertical spaces
Spacer(modifier = Modifier.height(16.dp))
// horizontal space
Spacer(modifier = Modifier.width(16.dp))
```

### Check only numbers
```
onValueChange = { input ->
        // Sadece rakamları kontrol etmek için
        if (input.all { it.isDigit() }) {
            age.value = input.toIntOrNull() ?: 0
        }
    }
```

### Concept of Arrange UI Elements
```
Column(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ){
	// UI Elements
}
```


### Create Dropdown Box
```
Box{
	Button(onClick = {}) {
		Text("Select")
		Icon(Icons.Default.ArrowDropDown,
			contentDescription = "Arrow Down")

	}
	DropdownMenu(expanded = true, onDismissRequest = {}) {
		DropdownMenuItem(
				text = { Text("Example 1") },
					 onClick = {})
		DropdownMenuItem(
				text = { Text("Example 2") },
					 onClick = {})
		DropdownMenuItem(
				text = { Text("Example 3") },
					 onClick = {})
		DropdownMenuItem(
				text = { Text("Milimeters") },
					 onClick = {})
	}
}
```

### viewModel add project
```
// inside of .toml ,version number may be outdated, chekout official website
androidx-lifecycle-viewmodel-compose = { module = "androidx.lifecycle:lifecycle-viewmodel-compose", version = "2.8.6" }

// inside of build.gradle
dependencies {
    implementation(libs.androidx.activity.compose)
    implementation(libs.androidx.lifecycle.viewmodel.compose)
    }

// inside of MainActivity
import androidx.lifecycle.viewmodel.compose.viewModel

```

### Add Maps dependencies
```
    implementation(libs.maps.compose)
    implementation(libs.play.services.location)

    implementation(libs.androidx.lifecycle.viewmodel.compose)
    implementation(libs.retrofit)
    implementation(libs.converter.gson)
    implementation(libs.androidx.navigation.compose)
```


### ADD Retrofit for Api Calls
```
[build.gradle.kts]
implementation(libs.retrofit2.retrofit)

[project file]
import retrofit2.Retrofit
etc.

```

### Add Restriction for GoogleCloud Service Api's
Inside of `console.cloud.google.com` follow theese steps
(Before these steps must create the api key on console)
Credentials > Select your api service > Android restrictions > ADD
this step need to two parameter
1. Package Name (inside the project top place placing like this package com.example)
2. Fingerprint {
	1) Open gradle window, (default placing right side, looks like tiny elephant icon
	2) Click Execute Gradle Task (looks like little cmd icon)
	3) write `gradle signingreport` and hit enter
	4) now wait a little time and see diferentt keys, we need to SHA1 key, thats the fingerprint
}