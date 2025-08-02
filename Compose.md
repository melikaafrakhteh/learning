# Jetpack Compose Tricks: Conditionally Applying Modifiers for Dynamic UIs

```
inline fun Modifier.conditional(
    condition: Boolean,
    ifTrue: Modifier.() -> Modifier,
    ifFalse: Modifier.() -> Modifier = { this },
): Modifier = if (condition) {
    then(ifTrue(Modifier))
} else {
    then(ifFalse(Modifier))
}


inline fun <T> Modifier.nullConditional(
    argument: T?,
    ifNotNull: Modifier.(T) -> Modifier,
    ifNull: Modifier.() -> Modifier = { this },
): Modifier {
    return if (argument != null) {
        then(ifNotNull(Modifier, argument))
    } else {
        then(ifNull(Modifier))
    }
}
```

[Link](https://proandroiddev.com/jetpack-compose-tricks-conditionally-applying-modifiers-for-dynamic-uis-e3fe5a119f45)

..........................................

# Focus in Jetpack Compose

```
 // First, get a reference to two focus requesters
 val (first, second) = FocusRequester.createRefs()

Column {
    // Down should take us to the third component
    TextField(
        ...
        modifier = Modifier.focusOrder(first) { down = second }
    )

    // Skip this one when moving in the "down" direction
    TextField(...)

    // Set the requester to tie them together
    TextField(
        ...
        modifier = Modifier.focusOrder(second)
    )
}

```

[Link](https://medium.com/google-developer-experts/focus-in-jetpack-compose-6584252257fe)

