### Reduce Recomposition for Images/Icons In Jetpack Compose

For Vector resources, we can use ImageVector instead of Painter, Because ImageVector is considered a stable type since it’s marked with @Immutable.

```
// From resources
Image(  
    imageVector = ImageVector.vectorResource(id = R.drawable.ic_launcher_background),
    ...
)

// For material icons
Image(
    imageVector = Icons.Default.ArrowForward,
    ...
)
```

 When designing reusable components prefer passing drawableResId/color types as param instead of Painter.

 .....................................................................
