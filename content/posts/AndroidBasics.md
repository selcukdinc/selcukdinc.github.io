+++
title = 'AndroidBasics'
date = 2024-10-13T11:29:49+03:00
draft = false
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