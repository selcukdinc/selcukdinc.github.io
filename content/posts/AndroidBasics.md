+++
title = 'AndroidBasics'
date = 2024-10-13T11:29:49+03:00
draft = false
ShowToc = true
+++

### Example of reusable variable
```
var loginButActive by  remember { mutableStateOf(true) }
```
### ?:  -  Elvis Operator
if variable goes null then give it default value and makes more tuf programs
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

### Check numbers on input
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


### Add Changable Texts in app
```
// For Text filed Usage like this
stringResource(R.string.Do_you_want_to_get_out)

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

### Add Restriction for GoogleCloud Service Api's

Inside of `console.cloud.google.com` follow theese steps
(Before these steps must create the api key on console)
Credentials > Select your api service > Android restrictions > ADD
this step need to two parameter
1. Package Name (inside the project top place placing like this package com.example)
2. Fingerprint {
	1) Open gradle window, (default placing right side, looks like tiny elephant icon)
	2) Click Execute Gradle Task (looks like little cmd icon)
	3) write `gradle signingreport` and hit enter
	4) now wait a little time and see diferentt keys, we need to SHA1 key, thats the fingerprint
}
# Ready To Useable Codes
## ExitDialog
```
        val context = LocalContext.current
        var showExitDialog by remember { mutableStateOf(false) }

        BackHandler {
            showExitDialog = true // Diyalog göster
        }

        if (showExitDialog) {
            AlertDialog(
                onDismissRequest = { showExitDialog = false },
                // Çıkmak İstiyor Musunuz?
                // Uygulamadan çıkmak istediğinize emin misiniz?
                title = { Text(text = stringResource(R.string.Do_you_want_to_get_out)) },
                text = { Text(text = stringResource(R.string.Are_you_sure_you_want_to_exit_the_app)) },
                confirmButton = {
                    Button(
                        onClick = {
                            // Uygulamadan çıkış
                            (context as Activity).finish()
                        }
                    ) {
                        Text(text = stringResource(R.string.Yes))
                    }
                },
                dismissButton = {
                    Button(onClick = { showExitDialog = false }) {
                        Text(text = stringResource(R.string.No))
                    }
                }
            )
        }
``` 


## Create Dropdown Box
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

## Create Toast With Clik the Button
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



# Dependencies

### Add Maps dependencies

```
    implementation(libs.maps.compose)
    implementation(libs.play.services.location)

    implementation(libs.androidx.lifecycle.viewmodel.compose)
    implementation(libs.retrofit)
    implementation(libs.converter.gson)
    implementation(libs.androidx.navigation.compose)
```

### Navigation Dependencies

```
[build.gradle.kts] :app
    implementation(libs.androidx.navigation.compose)
    implementation(libs.ui)
    implementation(libs.androidx.material)
    implementation(libs.ui.tooling.preview)
```

### ADD Retrofit for Api Calls

```
[build.gradle.kts]
implementation(libs.retrofit2.retrofit)

[project file]
import retrofit2.Retrofit
etc.

```

### ADD Room Database Dependencies
```
    val room = "2.6.0"
    // Room
    implementation("androidx.room:room-runtime:$room")
    implementation("androidx.room:room-ktx:$room")
    kapt("androidx.room:room-compiler:$room")
```
