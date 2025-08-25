[![Releases](https://img.shields.io/badge/Releases-Download-blue?style=for-the-badge&logo=github)](https://github.com/Samuka7700/Capsule/releases)

Capsule: Smooth Rounded Corners & Animations for Jetpack Compose
================================================================

![Capsule preview](https://raw.githubusercontent.com/google/material-design-icons/master/png/twotone/rounded_corner/2x/baseline_rounded_corner_black_48dp.png)

Topics: jetpack-compose

Quick links
-----------
- Releases (download and run the release file): https://github.com/Samuka7700/Capsule/releases
- Badge: [![Releases](https://img.shields.io/badge/Releases-Download-blue?style=for-the-badge&logo=github)](https://github.com/Samuka7700/Capsule/releases)

What Capsule does
-----------------
Capsule gives you smooth, customizable corner shapes and capsule-style components for Jetpack Compose. Use it to replace manual corner math, to unify corner handling across components, and to animate corner radius without layout jumps.

Key ideas:
- Provide composable shapes and modifiers for rounded corners.
- Smooth corner interpolation for animated states.
- Built-in capsule (pill) UI components.
- Low-level APIs that integrate with Material and Compose layouts.

Why use Capsule
----------------
- Keep UI consistent. One API controls corners across buttons, cards, chips.
- Animate corner radius in state changes without artifacts.
- Work with Compose shapes, draw caches, and clipping.
- Small API surface. You can add Capsule to an existing Compose project with minimal code.

Features
--------
- CapsuleShape: a shape that morphs between rectangles and pills.
- RoundedMask modifier: clip content with high-performance rounded masks.
- CornerAnimation: utilities to animate corner radius with spring or tween.
- CapsuleButton / CapsuleCard / CapsuleChip: ready-made components.
- Theme-aware defaults that match Material3 radius tokens.
- Extensions for Canvas and DrawScope to draw smooth capsule outlines.
- Kotlin + Compose targets Android SDK and Desktop Compose.

Install
-------
Add Capsule to your Gradle build. Example artifacts and coordinates are shown here:

Maven Central (Gradle Kotlin DSL)
```kotlin
repositories {
    mavenCentral()
}

dependencies {
    implementation("org.samuka:capsule:1.0.0")
}
```

Gradle Groovy
```groovy
repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.samuka:capsule:1.0.0'
}
```

Note: If you prefer a released binary, download the release asset and run or install it. Visit and download from:
https://github.com/Samuka7700/Capsule/releases
The release page contains packaged artifacts. Download the artifact you need (AAR/JAR/ZIP) and integrate it in your project or run the provided installer executable.

Quick start examples
--------------------

1) Simple capsule button
```kotlin
@Composable
fun DemoCapsuleButton(onClick: () -> Unit) {
    CapsuleButton(
        onClick = onClick,
        capsuleRadius = 24.dp, // pill style
        elevation = 4.dp,
    ) {
        Text("Capsule")
    }
}
```

2) Animated corner radius
```kotlin
@Composable
fun AnimatedCard(isActive: Boolean) {
    val targetRadius = if (isActive) 32.dp else 8.dp
    val radius by animateDpAsState(targetRadius, spring(dampingRatio = Spring.DampingRatioMediumBouncy))
    Card(shape = CapsuleShape(radius)) {
        Text("Stateful corners")
    }
}
```

3) Use RoundedMask modifier for content clipping
```kotlin
Box(
    modifier = Modifier
        .size(120.dp)
        .roundedMask(radius = 16.dp)
        .background(Color.Cyan)
) {
    // content will be clipped with a smooth rounded mask
}
```

API overview
------------
- CapsuleShape(radius: Dp) : Shape  
  Use with Surface, Card, or any composable that accepts a Shape.

- roundedMask(radius: Dp, antiAlias: Boolean = true) : Modifier  
  Apply to a Modifier to clip content. Works with hardware acceleration paths.

- CapsuleButton(onClick: () -> Unit, capsuleRadius: Dp, elevation: Dp, content: @Composable () -> Unit)  
  Prebuilt button with capsule radius and ripple integration.

- CornerAnimationSpec.spring(...) / CornerAnimationSpec.tween(...)  
  Animation helpers tuned for corner morphs to avoid jitter.

- drawCapsuleOutline(color: Color, stroke: Dp) : DrawScope extension  
  Fast outline drawing with support for stroke join modes and dash patterns.

Design notes
------------
- Capsule treats radius as a first-class property. You can animate the radius without forcing a full layout pass.
- The rounded mask uses a clip path optimized for Android Canvas and Skiko on Desktop.
- Capsule components opt into composition local values for content color and elevation so they work with MaterialTheme.

Customization guide
-------------------
- Colors: Use MaterialTheme.colorScheme or provide explicit Color parameters.
- Shadow and elevation: CapsuleButton and CapsuleCard accept elevation in Dp. Capsule uses shadow cache to reduce overdraw.
- Corner interpolation: Use CornerAnimationSpec to match your app motion settings.
- Stroke and border: drawCapsuleOutline exposes stroke width and cap/join settings.

Performance tips
----------------
- Prefer CapsuleShape for static clipping to let Compose cache the outline.
- For frequent radius updates, use RoundedMask modifier with antiAlias = false to reduce GPU cost, then re-enable antiAlias during idle states.
- Use hardware-accelerated draw calls. Capsule will fallback to CPU path only when necessary.

Compatibility matrix
--------------------
- Android: Compose 1.3+ recommended
- Kotlin: 1.7+ recommended
- Desktop: Compose for Desktop supported (same API surface)
- JVM: Library publishes AAR and JAR artifacts

Migration guide
---------------
From manual corner handling:
- Replace custom corner shapes with CapsuleShape(radius).
- If you used custom canvas clipping, migrate to roundedMask for consistent behavior.
- For animated corners, use CornerAnimationSpec instead of raw animate* APIs to avoid clipping artifacts.

Examples (expanded)
-------------------
Full composable example for a profile chip that animates to a capsule on selection:

```kotlin
@Composable
fun ProfileChip(name: String, selected: Boolean, onClick: () -> Unit) {
    val targetRadius = if (selected) 28.dp else 12.dp
    val radius by animateDpAsState(targetRadius, CornerAnimationSpec.spring())
    CapsuleChip(
        onClick = onClick,
        capsuleRadius = radius,
        leadingIcon = { Icon(Icons.Default.Person, contentDescription = null) }
    ) {
        Text(name)
    }
}
```

Testing
-------
- Unit test draw calls by capturing DrawScope operations.
- Use compose-test to assert shape presence and clipping behavior.
- Run visual tests on physical devices for different densities.

Releases & binaries
-------------------
Download release files and run them from the releases page:
https://github.com/Samuka7700/Capsule/releases

The releases page includes packaged AARs, JARs, and sample apps. Download the asset that matches your platform and follow the included README in the asset. If the release contains an executable installer or script, download that file and execute it on your machine.

Contributing
------------
- Fork the repo.
- Create a feature branch.
- Open a pull request with tests and examples.
- Follow the existing code style and use short, clear commit messages.
- Use the issue tracker for feature requests and bug reports.

License
-------
Capsule uses the MIT license. See LICENSE.md in this repository for details.

Community & Support
-------------------
- Open an issue for bugs and feature requests.
- Submit PRs for improvements and bug fixes.
- Check the Releases page for stable binaries and changelogs:
  https://github.com/Samuka7700/Capsule/releases

FAQ
---
Q: Will Capsule change my layout?
A: Capsule focuses on shapes and clipping. It does not change layout constraints. Use existing layout modifiers as before.

Q: Can I use it with Material3?
A: Yes. Capsule integrates with MaterialTheme and supports color and elevation tokens.

Q: Are there desktop builds?
A: Yes. The library publishes JVM artifacts that work with Compose for Desktop.

Illustrations and assets
------------------------
- Jetpack Compose logo and Material icons appear in examples and docs.
- Use the capsule preview image above to show a quick visual. Replace with animated GIFs or screenshots from your app for richer demos.

Changelog
---------
See the Releases page for change logs and downloadable assets:
[![Releases](https://img.shields.io/badge/Releases-View_Changes-orange?style=for-the-badge&logo=github)](https://github.com/Samuka7700/Capsule/releases)