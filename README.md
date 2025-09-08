# FaceUnity SDK for JitPack

[![](https://jitpack.io/v/sqmyeren/faceunity.svg)](https://jitpack.io/#sqmyeren/faceunity)

This repository now correctly publishes FaceUnity AAR files to JitPack, preserving native libraries and all required components.

## Installation

### Using with JitPack (v1.0.6+)

Add JitPack repository to your root build.gradle:

```gradle
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

Add the dependencies (both AAR files are required):

```gradle
dependencies {
    // Both libraries are required
    implementation 'com.github.sqmyeren:fu-core:v1.0.6'
    implementation 'com.github.sqmyeren:fu-model:v1.0.6'
}
```

### Using Local AAR Files

If JitPack is not available or you prefer local files:

```gradle
dependencies {
    implementation files('libs/fu-core-1.0.0.aar')
    implementation files('libs/fu-model-1.0.0.aar')
}
```

## What's Included

Version 1.0.6+ correctly distributes:
- ✅ All Java/Kotlin classes
- ✅ Native libraries (.so files) for all architectures
- ✅ JNI wrapper classes
- ✅ Resources and manifests
- ❌ BuildConfig classes (excluded to prevent conflicts)
- ❌ R classes (excluded to prevent conflicts)

## Important Notes

### For JitPack Users

When using JitPack, you must include **both** dependencies:
```gradle
implementation 'com.github.sqmyeren:fu-core:v1.0.6'
implementation 'com.github.sqmyeren:fu-model:v1.0.6'
```

Do NOT use the old single-JAR approach (versions before 1.0.6).

## Version History

- **v1.0.6** - Properly publish AAR files to JitPack (preserves native libraries)
- **v1.0.5** - (Deprecated) JAR approach - doesn't work with native libraries
- **v1.0.4** - (Deprecated) JAR approach - doesn't work with native libraries
- **v1.0.3** - (Deprecated) Initial JAR attempts

## Building from Source

```bash
# Clone the repository
git clone https://github.com/sqmyeren/faceunity.git
cd faceunity

# Build the JAR
./gradlew clean build

# The JAR will be in build/libs/
```

## License

This is a wrapper library for FaceUnity SDK. Please refer to FaceUnity's original license terms.

## Support

For issues related to this JitPack wrapper, please open an issue on [GitHub](https://github.com/sqmyeren/faceunity/issues).

For FaceUnity SDK specific issues, please contact FaceUnity support.
