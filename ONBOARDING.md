# Onboarding Guide - Android JL OTA SDK

Welcome! This guide will help new developers get started with the Android JL OTA SDK project.

## Table of Contents

1. [Project Overview](#project-overview)
2. [Getting Started](#getting-started)
3. [Development Environment Setup](#development-environment-setup)
4. [Project Structure](#project-structure)
5. [Core Concepts](#core-concepts)
6. [Development Workflow](#development-workflow)
7. [Testing](#testing)
8. [Troubleshooting](#troubleshooting)
9. [Additional Resources](#additional-resources)

---

## Project Overview

**Android JL OTA SDK** is a Bluetooth OTA (Over-The-Air) firmware update solution for JieLi devices.

### Key Features

- ✅ **BLE (Bluetooth Low Energy)** and **SPP (Serial Port Profile)** communication support
- ✅ TWS earbuds and single device OTA support
- ✅ Multiple device simultaneous upgrade support
- ✅ Android 14+ compatibility
- ✅ x86, x86_64, ARM platform support

### Version Information

- **Latest Version**: APP_V1.8.1_SDK_V1.10.0
- **Release Date**: August 11, 2025
- **Maintainer**: 钟卓成 (Zhong Zhuocheng)

---

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

#### Required

- **Android Studio**: Arctic Fox (2020.3.1) or later
- **JDK**: 11 or later
- **Gradle**: 7.0 or later
- **Android SDK**:
  - Minimum SDK: API 21 (Android 5.0)
  - Target SDK: API 34 (Android 14)
  - Compile SDK: API 34

#### Recommended

- Git 2.30 or later
- Android device (for real Bluetooth testing)
- JieLi device (for OTA testing)

### Clone Repository

```bash
git clone https://github.com/Jieli-Tech/Android-JL_OTA.git
cd Android-JL_OTA
```

---

## Development Environment Setup

### 1. Open Project in Android Studio

1. Launch Android Studio
2. Select **File → Open**
3. Navigate to `Android-JL_OTA/code/JL_OTA_Android_V1.8.1_SDK_V1.10.0`
4. Wait for Gradle sync to complete

### 2. Verify SDK Library Setup

The project uses the following SDK libraries:

```
libs/
├── jl_bt_ota_V1.10.0_10931-release.aar
└── jl_bt_ota_V1.10.0_10932-release.aar
```

### 3. Permissions Setup

The app requires the following permissions to function properly:

- `BLUETOOTH` / `BLUETOOTH_ADMIN` (Android 11 and below)
- `BLUETOOTH_SCAN` / `BLUETOOTH_CONNECT` (Android 12+)
- `ACCESS_FINE_LOCATION` (for Bluetooth scanning)
- `READ_EXTERNAL_STORAGE` / `WRITE_EXTERNAL_STORAGE`
- `MANAGE_EXTERNAL_STORAGE` (Android 11+)

### 4. Run First Build

```bash
cd code/JL_OTA_Android_V1.8.1_SDK_V1.10.0
./gradlew clean build
```

Or in Android Studio:
- **Build → Make Project** (Ctrl+F9 / Cmd+F9)

---

## Project Structure

```
Android-JL_OTA/
├── apk/                          # Release APK files
│   └── JLOTA_V1.8.1_10807-debug.apk
├── code/                         # Source code
│   └── JL_OTA_Android_V1.8.1_SDK_V1.10.0/
│       ├── otasdk/              # Main app module
│       │   ├── src/
│       │   │   └── main/
│       │   │       ├── java/com/jieli/otasdk/
│       │   │       │   ├── activities/     # UI Activities
│       │   │       │   ├── fragments/      # UI Fragments
│       │   │       │   ├── tool/           # Core OTA logic
│       │   │       │   ├── viewmodel/      # MVVM ViewModels
│       │   │       │   ├── adapter/        # RecyclerView Adapters
│       │   │       │   └── util/           # Utility classes
│       │   │       ├── res/                # Resources (layouts, strings, etc.)
│       │   │       └── AndroidManifest.xml
│       │   └── build.gradle
│       ├── build.gradle
│       └── settings.gradle
├── doc/                          # Documentation
│   ├── JieLi_OTA_SDK_Android_Development_Doc.url
│   └── 杰理OTA外接库(Android)开发文档.url
├── libs/                         # SDK Libraries
│   ├── jl_bt_ota_V1.10.0_10931-release.aar
│   └── jl_bt_ota_V1.10.0_10932-release.aar
├── README.md                     # Project overview
└── LICENSE                       # License information
```

### Key Directory Descriptions

| Directory | Description |
|-----------|-------------|
| `otasdk/tool/ota/` | Core OTA update logic (OTAManager, communication handling) |
| `otasdk/viewmodel/` | MVVM architecture ViewModels |
| `otasdk/activities/` | Main UI screens |
| `otasdk/util/` | Helper functions and utilities |

---

## Core Concepts

### 1. OTA (Over-The-Air) Updates

OTA is a technology for wirelessly updating device firmware.

**Supported Protocols:**
- **BLE (Bluetooth Low Energy)**: Default, low-power Bluetooth communication
- **SPP (Serial Port Profile)**: Classic Bluetooth serial communication

### 2. OTA Workflow

```
1. Device Discovery & Connection
   ↓
2. Firmware File Selection
   ↓
3. OTA Manager Configuration
   ↓
4. Firmware Transfer
   ↓
5. Device Reboot & Update Application
   ↓
6. Update Completion Verification
```

### 3. OTA Configuration Options

Configure via `BluetoothOTAConfigure` class:

```kotlin
val bluetoothOption = BluetoothOTAConfigure()

// Select communication method (prefer BLE)
bluetoothOption.priority = BluetoothOTAConfigure.PREFER_BLE

// Use custom reconnection method
bluetoothOption.isUseReconnect = !JL_Constant.NEED_CUSTOM_RECONNECT_WAY

// Enable device authentication (confirm with firmware engineer)
bluetoothOption.isUseAuthDevice = JL_Constant.IS_NEED_DEVICE_AUTH

// MTU settings (Deprecated)
bluetoothOption.mtu = BluetoothConstant.BLE_MTU_MIN
bluetoothOption.isNeedChangeMtu = false

// Use JieLi server (currently not supported)
bluetoothOption.isUseJLServer = false

// Apply configuration to OTAManager
OTAManager.getInstance().configure(bluetoothOption)
```

### 4. Logging System

```java
// Configure log output
JL_Log.setIsLog(BuildConfig.DEBUG)

// Configure log file saving
JL_Log.setIsSaveLogFile(context, BuildConfig.DEBUG)
```

**Log File Location**: `Android/data/[package_name]/files/logcat/`

**Log File Format**: `ota_log_app_[timestamp].txt`

---

## Development Workflow

### 1. Developing New Features

#### Step 1: Create Branch
```bash
git checkout -b feature/your-feature-name
```

#### Step 2: Write Code
- Follow MVVM pattern
- Adhere to Kotlin coding conventions
- Add comments for important logic

#### Step 3: Test
```bash
./gradlew test
./gradlew connectedAndroidTest
```

#### Step 4: Commit and Push
```bash
git add .
git commit -m "feat: Add your feature description"
git push origin feature/your-feature-name
```

### 2. Bug Fixes

#### Step 1: Review Issue
- Check bug report in GitHub Issues
- Understand reproduction steps

#### Step 2: Analyze Logs
- Check logs in `Android/data/com.jieli.otasdk/files/logcat/`
- Analyze error stack traces

#### Step 3: Fix and Verify
- Apply fix
- Test reproduction
- Run regression tests

### 3. Code Review Guidelines

Include in Pull Requests:
- Summary of changes
- Test results
- Screenshots (for UI changes)
- Breaking changes (if any)

---

## Testing

### 1. Unit Tests

```bash
./gradlew test
```

### 2. Integration Tests

```bash
./gradlew connectedAndroidTest
```

### 3. Manual Testing

#### OTA Update Test Procedure

1. **Install Test APK**
   ```bash
   adb install apk/JLOTA_V1.8.1_10807-debug.apk
   ```

2. **Prepare Firmware File**
   - Copy firmware file to:
   ```
   /Android/data/com.jieli.otasdk/files/upgrade/
   ```

3. **Connect Device**
   - Enable Bluetooth in app
   - Search and connect to target JieLi device

4. **Execute OTA**
   - Select firmware file
   - Start OTA
   - Monitor progress

5. **Verify Results**
   - Confirm update success
   - Review log files

### 4. Test Checklist

- [ ] BLE connection test
- [ ] SPP connection test
- [ ] Single device OTA
- [ ] TWS earbuds OTA
- [ ] Multiple device OTA
- [ ] Permission handling (Android 14)
- [ ] Reconnection mechanism
- [ ] Error handling

---

## Troubleshooting

### Common Issues

#### 1. Bluetooth Connection Failure

**Symptom**: Device discovered but connection fails

**Solutions**:
- Verify permissions by Android version
  - Android 12+: `BLUETOOTH_SCAN`, `BLUETOOTH_CONNECT` required
  - Android 6-11: `ACCESS_FINE_LOCATION` required
- Confirm location services (GPS) enabled
- Restart app

#### 2. OTA Transfer Failure

**Symptom**: Failure during update process

**Solutions**:
- Verify MTU settings
- Check device distance (within 1m recommended)
- Check battery level (device)
- Analyze log files

#### 3. Android 14 Storage Permission Issues

**Symptom**: Cannot access firmware files

**Solutions**:
- Request `MANAGE_EXTERNAL_STORAGE` permission
- Verify Scoped Storage API usage
- Use app-specific directory

#### 4. BLE Transfer Speed Degradation

**Symptom**: Abnormally slow data transfer

**Solutions**:
- Use SDK V1.9.3 or later (fixed)
- Check Connection Interval
- Check packet transmission status in logs

### Log Analysis

1. **Log File Location**:
   ```
   /Android/data/com.jieli.otasdk/files/logcat/ota_log_app_[timestamp].txt
   ```

2. **Key Log Keywords**:
   - `OTA_START`: OTA started
   - `OTA_PROGRESS`: Progress
   - `OTA_SUCCESS`: Success
   - `OTA_FAILED`: Failure
   - `ERROR`: Error occurred

3. **Extract Logs**:
   ```bash
   adb pull /sdcard/Android/data/com.jieli.otasdk/files/logcat/ ./logs/
   ```

### Issue Reporting

If problems persist:

1. **Brief problem description** (required)
2. **Provide latest log file** (required)
3. Specify occurrence time
4. Attach screenshots or video

GitHub Issues: https://github.com/Jieli-Tech/Android-JL_OTA/issues

---

## Additional Resources

### Official Documentation

- [JieLi OTA SDK Development Documentation (Chinese)](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/index.html)
- [FAQ (Frequently Asked Questions)](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/other/qa.html)

### Reference Materials

- **Android Bluetooth Guide**: https://developer.android.com/guide/topics/connectivity/bluetooth
- **Android Permissions Guide**: https://developer.android.com/guide/topics/permissions/overview
- **MVVM Architecture**: https://developer.android.com/topic/architecture

### Community

- GitHub Issues: Technical questions and bug reports
- Official documentation Q&A section

---

## Quick Reference

### Key Commands

```bash
# Clone project
git clone https://github.com/Jieli-Tech/Android-JL_OTA.git

# Build
./gradlew clean build

# Install debug APK
adb install -r apk/JLOTA_V1.8.1_10807-debug.apk

# View logs
adb logcat | grep "JL_OTA"

# Extract log files
adb pull /sdcard/Android/data/com.jieli.otasdk/files/logcat/
```

### Key File Paths

| Description | Path |
|-------------|------|
| OTA Manager | `otasdk/src/main/java/com/jieli/otasdk/tool/ota/OTAManager` |
| Bluetooth Config | `otasdk/src/main/java/com/jieli/otasdk/tool/ota/BluetoothOTAConfigure` |
| Main Activity | `otasdk/src/main/java/com/jieli/otasdk/activities/` |
| Firmware Storage | `/Android/data/com.jieli.otasdk/files/upgrade/` |
| Log Files | `/Android/data/com.jieli.otasdk/files/logcat/` |

---

## Next Steps

Onboarding complete? Try these next:

1. ✅ Build and run demo app
2. ✅ Test OTA with real device
3. ✅ Explore codebase (especially `tool/ota/` directory)
4. ✅ Choose and solve first issue
5. ✅ Submit first Pull Request

**Questions? Feel free to ask via Issues anytime!**

---

Last Updated: 2025-11-09
