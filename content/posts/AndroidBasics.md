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
