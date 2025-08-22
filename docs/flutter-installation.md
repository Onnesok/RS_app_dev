# Flutter Installation Guide

This guide will help you install Flutter on your system and set up your development environment.

## Prerequisites

Before installing Flutter, make sure you have:
- A computer with at least 8GB RAM (16GB recommended)
- At least 10GB of free disk space
- Windows 10 or later (for Windows users)

## Step 1: Download Flutter SDK

### Windows
1. Go to [Flutter Downloads](https://docs.flutter.dev/get-started/install/windows)
2. Download the Flutter SDK zip file
3. Extract the zip file to a location like `C:\src\flutter`
4. **Important**: Don't extract to a folder that requires elevated privileges (like `C:\Program Files`)

### macOS
1. Go to [Flutter Downloads](https://docs.flutter.dev/get-started/install/macos)
2. Download the Flutter SDK zip file
3. Extract to a location like `/Users/yourname/development/flutter`

### Linux
1. Go to [Flutter Downloads](https://docs.flutter.dev/get-started/install/linux)
2. Download the Flutter SDK tar file
3. Extract to a location like `/home/yourname/development/flutter`

## Step 2: Add Flutter to PATH

### Windows
1. Open System Properties → Advanced → Environment Variables
2. Under "User variables", find and select "Path"
3. Click "Edit" and add the path to flutter\bin
   ```
   C:\src\flutter\bin
   ```
4. Click "OK" to save

### macOS/Linux
Add Flutter to your PATH by editing your shell profile:

```bash
# For bash (add to ~/.bash_profile or ~/.bashrc)
export PATH="$PATH:/path/to/flutter/bin"

# For zsh (add to ~/.zshrc)
export PATH="$PATH:/path/to/flutter/bin"
```

## Step 3: Install Required Dependencies

### Windows
1. **Git for Windows**: Download from [git-scm.com](https://git-scm.com/download/win)
2. **Android Studio**: Download from [developer.android.com](https://developer.android.com/studio)

### macOS
1. **Xcode**: Install from Mac App Store (for iOS development)
2. **Android Studio**: Download from [developer.android.com](https://developer.android.com/studio)

### Linux
1. **Android Studio**: Download from [developer.android.com](https://developer.android.com/studio)

## Step 4: Verify Installation

Open a terminal/command prompt and run:

```bash
flutter doctor
```

This command checks your environment and displays a report of the status of your Flutter installation.

### Understanding flutter doctor output:

```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 3.x.x, on Microsoft Windows [Version 10.x.x])
[✓] Windows Version (Installed version of Windows is version 10 or higher)
[✓] Android toolchain - develop for Android devices (Android SDK version 33.x.x)
[✓] Chrome - develop for the web
[✓] Visual Studio - develop for Windows (Visual Studio Community 2022 17.x.x)
[✓] Android Studio (version 2022.x.x)
[✓] VS Code (version 1.x.x)
[✓] Connected device (3 available)
[✓] Network resources

• No issues found!
```

## Step 5: Install an IDE

### Option 1: Visual Studio Code (Recommended for beginners)
1. Download VS Code from [code.visualstudio.com](https://code.visualstudio.com/)
2. Install the Flutter extension:
   - Open VS Code
   - Go to Extensions (Ctrl+Shift+X)
   - Search for "Flutter"
   - Install the Flutter extension by Dart Code

### Option 2: Android Studio
1. Open Android Studio
2. Go to Plugins
3. Search for "Flutter"
4. Install the Flutter plugin
5. Restart Android Studio

## Step 6: Set Up Android Development

### Install Android SDK
1. Open Android Studio
2. Go to Tools → SDK Manager
3. Install the following:
   - Android SDK Platform-Tools
   - Android SDK Build-Tools
   - At least one Android SDK Platform (API level 21 or higher)
   - Android SDK Command-line Tools

### Create Android Virtual Device (AVD)
1. In Android Studio, go to Tools → AVD Manager
2. Click "Create Virtual Device"
3. Select a device (e.g., Pixel 4)
4. Select a system image (e.g., API 33)
5. Click "Finish"

## Step 7: Create Your First Flutter Project

### Using Command Line
```bash
# Create a new Flutter project
flutter create my_first_app

# Navigate to the project directory
cd my_first_app

# Run the app
flutter run
```

### Using VS Code
1. Open VS Code
2. Press Ctrl+Shift+P (Cmd+Shift+P on macOS)
3. Type "Flutter: New Project"
4. Select a project location
5. Enter a project name
6. Wait for the project to be created
7. Press F5 to run the app

### Using Android Studio
1. Open Android Studio
2. Click "New Flutter Project"
3. Enter project details
4. Click "Finish"
5. Click the "Run" button

## Step 8: Test Your Setup

### Run on Android Emulator
1. Start your Android emulator
2. Run `flutter run` in your project directory
3. The app should launch on the emulator

### Run on Web Browser
```bash
flutter run -d chrome
```

### Run on Physical Device
1. Enable Developer Options on your Android device
2. Enable USB Debugging
3. Connect your device via USB
4. Run `flutter run`

## Common Issues and Solutions

### Issue: "flutter command not found"
**Solution**: Make sure Flutter is added to your PATH and restart your terminal.

### Issue: "Android SDK not found"
**Solution**: 
1. Install Android Studio
2. Install Android SDK through SDK Manager
3. Set ANDROID_HOME environment variable

### Issue: "No connected devices"
**Solution**:
1. Start an Android emulator, or
2. Connect a physical device with USB debugging enabled

### Issue: "Gradle build failed"
**Solution**:
1. Check your internet connection
2. Try `flutter clean` then `flutter pub get`
3. Update your Flutter SDK: `flutter upgrade`

## Updating Flutter

To update Flutter to the latest version:

```bash
flutter upgrade
```

To switch to a different Flutter channel:

```bash
flutter channel stable  # or beta, master
flutter upgrade
```

## Verifying Everything Works

Run this command to ensure everything is set up correctly:

```bash
flutter doctor -v
```

You should see all checkmarks (✓) and no issues.

## Next Steps

After successful installation:

1. **Learn Flutter Basics**: Read the [Flutter Basics](./flutter-basics.md) guide
2. **Create Your First App**: Follow the Flutter "Hello World" tutorial
3. **Explore Widgets**: Learn about different Flutter widgets
4. **Practice**: Build small projects to get familiar with Flutter

## Getting Help

If you encounter issues:

1. **Flutter Documentation**: [docs.flutter.dev](https://docs.flutter.dev)
2. **Flutter GitHub Issues**: [github.com/flutter/flutter/issues](https://github.com/flutter/flutter/issues)
3. **Stack Overflow**: Search for Flutter-related questions
4. **Flutter Community**: Join Flutter Discord or Reddit communities

## Summary

You've successfully installed Flutter! The key steps were:

1. ✅ Download and extract Flutter SDK
2. ✅ Add Flutter to PATH
3. ✅ Install dependencies (Git, Android Studio)
4. ✅ Run `flutter doctor` to verify setup
5. ✅ Install an IDE (VS Code or Android Studio)
6. ✅ Set up Android development environment
7. ✅ Create and run your first Flutter project

Now you're ready to start building amazing Flutter apps!
