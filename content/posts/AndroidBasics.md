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