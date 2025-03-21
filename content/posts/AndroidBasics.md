+++
title = 'AndroidBasics'
date = 2024-10-13T11:29:49+03:00
draft = false
ShowToc = true
+++
### Examle Flow  Variable
```
// ViewModel
    private val _strings = MutableStateFlow<List<String, Any>>(emptyList())
    val strings: StateFlow<List<String, Any>> get() = _strings
// Screen
    val listedReports = exploreListViewModel.reports.collectAsState()
```
### Example Of IO Thread Function for ViewModel
```
fun fetchSomething() {
        viewModelScope.launch (Dispatchers.IO){
            // Fetching... on IO Thread (Faster and relase ui thread)
        }
    }
```
### Example of reusable variable inside of screen
```
var loginButActive by  remember { mutableStateOf(true) }
var nameText by remember { mutableStateOf("") }
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

### Example Column
```
Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.Top,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        // TODO
    }
```

### Example Text
```
Text(
    text = stringResource(R.string.Hi),
    fontSize = 24.sp,
    fontWeight = FontWeight.Bold,
    color = MaterialTheme.colorScheme.onBackground
)
```

### Example Outlined Text
```
OutlinedTextField(
    value = variable,
    onValueChange = { variable = it },
    label = { Text(text = stringResource(R.string.variable)) },
    modifier = Modifier
        .fillMaxWidth()
        .padding(8.dp)
    )
``` 

### Example Preview
```
@Preview(showBackground = true)
@Composable
fun LoginPreview() {
    val authViewModel: AuthViewModel = viewModel()
    LoginScreen(
        authViewModel = authViewModel,
        onNavigateToSignUp = {
            // TODO
        }
    ){
        // TODO
    }
}
```
### Example Clickable Text
```
Text(text = "Sample Text",
            modifier = Modifier.clickable { onNavigateToSignUp() }
        )
```
### Example Launch Effect
```
LaunchedEffect(TextObserableState){
        when (TextObserableState) {
            is Result.Success -> {
                onSignInSuccess()
                Toast.makeText(context, "Successful Return", Toast.LENGTH_SHORT).show()
            }
            is Result.Error -> {
                Toast.makeText(context, "Something Went Wrong", Toast.LENGTH_SHORT).show()
            }
            else -> Unit
        }
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
## Text Field and Button Inside the Alert Dialog
```
if (showDialogCreateRoom){
    AlertDialog( onDismissRequest = { showDialogCreateRoom = true },
        title = { Text(text = stringResource(R.string.Create_a_new_room)) },
        text={
            OutlinedTextField(
                value = name,
                onValueChange = { name = it },
                singleLine = true,
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(8.dp)
            )
        }, confirmButton = {
            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(8.dp),
                horizontalArrangement = Arrangement.SpaceBetween
            ) {
                Button(
                    onClick = {
                        if (name.isNotBlank()) {
                            showDialogCreateRoom = false
                            roomViewModel.createRoom(name)
                            roomViewModel.loadRooms()
                        }
                    }
                ) {
                    Text(text = stringResource(R.string.Add))
                }
                Button(
                    onClick = { showDialogCreateRoom = false }
                ) {
                    Text(text = stringResource(R.string.Cancel))
                }
            }
        })
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
