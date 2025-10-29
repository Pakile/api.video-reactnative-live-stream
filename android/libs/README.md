# rtmpdroid with 16KB Page Size Support

This directory contains a custom build of rtmpdroid (v1.3.0) with support for Android 15+ devices that use 16KB page size.

## Files

- `rtmpdroid-16kb.jar` - Kotlin/Java classes extracted from the AAR
- Native libraries (`.so` files) are located in `../src/main/jniLibs/`

## Build Information

Built from: https://github.com/apivideo/api.video-rtmpdroid

### Changes from official version:
- Added linker flag: `-Wl,-z,max-page-size=16384` in CMakeLists.txt
- Updated to Gradle 8.9
- Updated target SDK to 35 (Android 15)
- Updated compile SDK to 35

### Native libraries included:
- `arm64-v8a/librtmpdroid.so` (4.5 MB)
- `armeabi-v7a/librtmpdroid.so` (3.5 MB)
- `x86/librtmpdroid.so` (4.1 MB)
- `x86_64/librtmpdroid.so` (4.7 MB)

## How to Update

If you need to rebuild rtmpdroid:

1. Clone and build rtmpdroid with 16KB page size support:
```bash
cd /path/to/api.video-rtmpdroid
./gradlew :lib:assemblePackedRelease publishToMavenLocal
```

2. Extract files from the AAR:
```bash
cd /tmp
unzip -o ~/.m2/repository/video/api/rtmpdroid/VERSION/rtmpdroid-VERSION-packed.aar

# Copy JAR
cp classes.jar /path/to/react-native-livestream/android/libs/rtmpdroid-16kb.jar

# Copy native libraries
cp -r jni/* /path/to/react-native-livestream/android/src/main/jniLibs/
```

3. Rebuild react-native-livestream:
```bash
cd /path/to/react-native-livestream/example/android
./gradlew assembleDebug
```

## Why bundled directly?

React Native libraries (AAR modules) cannot depend on local AAR files due to Gradle limitations. 
Instead, we extract and bundle the compiled code directly into the source tree.

This approach:
- ✅ Works on any machine without Maven Local setup
- ✅ Can be committed to git
- ✅ No external dependencies needed for building
- ✅ Fully portable across development machines

