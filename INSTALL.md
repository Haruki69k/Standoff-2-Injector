# ReVrax v2.0 — Installation & Build Guide

## System Requirements

### For Termux Build (Phone Only)
- Android 10+ (API 29+)
- Termux from **F-Droid** (Play Store version is broken)
- 2GB free storage
- Root access (Magisk / KernelSU / SuperSU)
- Architecture: ARM64, ARMv7, x86, or x86_64

### For PC Build
- Windows 10/11, Linux, or macOS
- Android Studio Hedgehog (2023.1.1) or newer
- Android SDK API 34
- Android NDK r26.1.10909125
- CMake 3.22.1+
- 8GB RAM minimum

---

## Method 1: Termux Build (No PC Required)

### Step 1: Install Termux
Download Termux from F-Droid:
```
https://f-droid.org/packages/com.termux/
```
**Do NOT use Play Store version — it is outdated and broken.**

### Step 2: Setup Environment
Open Termux and run:
```bash
pkg update && pkg upgrade -y
pkg install git cmake make clang ndk-sysroot aapt apksigner zip unzip -y
```

### Step 3: Download ImGui (Required)
```bash
cd ~
# Extract ReVrax_v2.0.zip first
cd ReVrax_v2.0
git clone --depth 1 https://github.com/ocornut/imgui.git jni/imgui
```

### Step 4: Build Full APK
```bash
chmod +x build_termux.sh
./build_termux.sh
```

This will:
1. Build native library for ARM64
2. Compile resources with aapt
3. Package APK
4. Sign APK with debug keystore

### Step 5: Install
```bash
adb install ReVrax_v2.0.apk
# Or manually install from file manager
```

---

## Method 2: Android Studio (PC — Recommended)

### Step 1: Install Prerequisites
1. Download and install [Android Studio](https://developer.android.com/studio)
2. Open SDK Manager → SDK Platforms → Install **Android 14 (API 34)**
3. Open SDK Manager → SDK Tools → Install:
   - **NDK r26.1.10909125**
   - **CMake 3.22.1**
   - **Android SDK Build-Tools**

### Step 2: Open Project
1. Extract `ReVrax_v2.0.zip`
2. Open Android Studio
3. **File → Open → Select `ReVrax_v2.0` folder**
4. Wait for Gradle sync (may take 5-10 minutes first time)

### Step 3: Configure NDK Path
Edit `local.properties`:
```properties
sdk.dir=C:\Users\YOUR_NAME\AppData\Local\Android\Sdk
ndk.dir=C:\Users\YOUR_NAME\AppData\Local\Android\Sdk\ndk\26.1.10909125
```
*(On Linux/macOS use forward slashes: `/home/user/Android/Sdk`)*

### Step 4: Download ImGui
```bash
cd ReVrax_v2.0
git clone --depth 1 https://github.com/ocornut/imgui.git jni/imgui
```

### Step 5: Build APK
**Via Android Studio:**
```
Build → Build Bundle(s) / APK(s) → Build APK(s)
```

**Via command line:**
```bash
./gradlew assembleRelease
```

### Step 6: Output
```
app/build/outputs/apk/release/app-release.apk
```

---

## Method 3: PC Command Line (NDK)

### Linux / macOS:
```bash
export ANDROID_SDK=$HOME/Android/Sdk
chmod +x build_pc.sh
./build_pc.sh
```

### Windows (Git Bash or WSL):
```bash
export ANDROID_SDK="/c/Users/YOUR_NAME/AppData/Local/Android/Sdk"
chmod +x build_pc.sh
./build_pc.sh
```

---

## Post-Build: Adding ImGui

ImGui is **NOT included** in this archive due to licensing/size. Download it:

```bash
cd ReVrax_v2.0/jni
git clone --depth 1 https://github.com/ocornut/imgui.git
```

Required ImGui files:
```
imgui/imgui.cpp
imgui/imgui_demo.cpp
imgui/imgui_draw.cpp
imgui/imgui_tables.cpp
imgui/imgui_widgets.cpp
imgui/imgui.h
imgui/imgui_internal.h
imgui/imconfig.h
imgui/backends/imgui_impl_android.cpp
imgui/backends/imgui_impl_android.h
imgui/backends/imgui_impl_opengl3.cpp
imgui/backends/imgui_impl_opengl3.h
imgui/backends/imgui_impl_vulkan.cpp
imgui/backends/imgui_impl_vulkan.h
```

---

## Optional: Advanced Dependencies

### IL2CPP Resolver (Recommended for Production)
For runtime function resolution when game updates:
- Download `il2cpp-api-functions.h` from il2cppdumper repository
- Place in `jni/il2cpp/` directory
- Integrate into `main.cpp` for dynamic offset resolution

### Hook Framework (Recommended for Production)
For reliable inline hooking:
- **Dobby**: `git clone https://github.com/jmpews/Dobby.git`
- Integrate into `jni/utils/memory.cpp`
- Replace placeholder hooks with Dobby hooks

---

## Troubleshooting

### "libil2cpp.so not found"
- Ensure Standoff 2 v0.39.2 is installed
- Launch game at least once before injection

### "eglCreateWindowSurface failed"
- Grant overlay permission in system settings
- Restart app after granting permission

### "CMake not found" (Termux)
```bash
pkg install cmake -y
```

### "NDK not found" (PC)
- Verify NDK path in `local.properties`
- Reinstall NDK via SDK Manager

### "imgui not found"
```bash
git clone --depth 1 https://github.com/ocornut/imgui.git jni/imgui
```

### Build fails on x86/x86_64
- Make sure NDK supports your target architecture
- Check `abiFilters` in `build.gradle`

---

## Safety Tips

| Risk Level | Recommendation |
|-----------|----------------|
| 🟢 SAFE | Can use on main account |
| 🟡 MEDIUM | Use on alt account first |
| 🔴 DANGEROUS | High ban risk, test thoroughly |
| ⚫ EXTREME | Almost guaranteed ban, use fresh account |

- **Always use spoofing** before first launch
- **Use Ban Reset** if previously banned
- **Avoid EXTREME features** on accounts you care about
- **Update offsets** when game updates

---

## Support

For issues and updates:
- Check README.md for architecture and renderer compatibility
- Verify all dependencies are installed
- Ensure correct NDK version (r26.1.10909125)

**Remember: This tool is for educational purposes. Use at your own risk.**
