# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

FaceUnity SDK is a Java library project for face recognition and beauty features. It packages FaceUnity's Android AAR libraries (fu-core and fu-model) into a distributable JAR for use with JitPack.

## Build System

The project uses Gradle 8.4 with Java 8 compatibility settings. The Gradle wrapper is included for consistent builds across environments.

## Key Commands

### Building the Project
```bash
# Clean and build the library JAR
./gradlew clean build

# Build only (without clean)
./gradlew build

# Assemble the JAR without running tests
./gradlew assemble

# Create source JAR
./gradlew sourceJar
```

### Testing
```bash
# Run all tests
./gradlew test

# Run tests with detailed output
./gradlew test --info
```

### Publishing
```bash
# Publish to local Maven repository (~/.m2/repository)
./gradlew publishToMavenLocal

# Generate POM file for Maven publication
./gradlew generatePomFileForMavenPublication

# Publish all publications
./gradlew publish
```

### Dependency Management
```bash
# View project dependencies
./gradlew dependencies

# View dependency tree for specific configuration
./gradlew dependencies --configuration runtimeClasspath

# Check for dependency updates
./gradlew dependencyInsight --dependency <dependency-name>
```

### Cleaning
```bash
# Clean build artifacts
./gradlew clean

# Clean and rebuild
./gradlew clean build
```

## Project Architecture

### Directory Structure
- `/libs/` - Contains the FaceUnity AAR libraries:
  - `fu-core-1.0.0.aar` - Core FaceUnity functionality (34MB)
  - `fu-model-1.0.0.aar` - FaceUnity model files (50MB)
- `/build/` - Generated build artifacts (gitignored)
- `/.gradle/` - Gradle cache and metadata (gitignored)
- `/gradle/wrapper/` - Gradle wrapper configuration

### Build Configuration
- **Group ID**: `com.faceunity`
- **Artifact ID**: `faceunity-sdk`
- **Current Version**: 1.0.4
- **Java Version**: 1.8 (source and target compatibility)
- **Gradle Version**: 8.4

### Key Files
- `build.gradle` - Main build configuration with Maven publishing setup
- `settings.gradle` - Project name configuration
- `gradle.properties` - JVM and build optimization settings

## Publishing to JitPack

This library is designed for distribution via JitPack. To release a new version:

1. Update version in `build.gradle`
2. Commit and push changes
3. Create and push a Git tag:
   ```bash
   git tag -a v1.0.x -m "Release version 1.0.x"
   git push origin v1.0.x
   ```
4. JitPack will automatically build from the tag

## JitPack Integration

Users can include this library in their projects by adding:

### Gradle
```gradle
repositories {
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation 'com.github.sqmyeren:faceunity:v1.0.4'
}
```

### Maven
```xml
<repository>
    <id>jitpack.io</id>
    <url>https://jitpack.io</url>
</repository>

<dependency>
    <groupId>com.github.sqmyeren</groupId>
    <artifactId>faceunity</artifactId>
    <version>v1.0.4</version>
</dependency>
```

## Claude Settings

The project includes Claude AI permissions in `.claude/settings.local.json` that allow:
- Gradle wrapper operations
- Git operations (add, commit, tag, push)
- Java execution
- File operations (chmod, curl, unzip)
- Web fetching from jitpack.io

## Development Notes

### AAR to JAR Packaging
The build process extracts and includes AAR contents into the final JAR. This is handled by the custom jar task configuration that uses `zipTree` to extract AAR files.

### Duplicate Strategy
The JAR task uses `DuplicatesStrategy.EXCLUDE` to handle any duplicate files when merging AAR contents.

### Source Compatibility
Maintains Java 8 compatibility for broader project support while using Gradle 8.4 and JVM 21 for the build environment.
