# Android Debug Bridge (ADB) Guide

**Android Debug Bridge (ADB)** is a versatile command-line tool that facilitates communication between a computer and an Android device. ADB allows for installing and debugging applications, transferring files, accessing device internals, and more. It’s part of the Android SDK (Software Development Kit) and works with both physical and emulated devices.

---

## Components of ADB

|Component|Description|
|---|---|
|**Client**|Runs on the host machine and invoked from the terminal using the `adb` command.|
|**Daemon (adbd)**|Background process running on the device to execute commands.|
|**Server**|Background process on the host that manages communication between the client and daemon.|

---

## ADB Connection Process

1. **Client Initialization**
    - Executing an `adb` command checks for a running ADB server.
    - If none is found, a server is started on TCP port `5037`.
2. **Server Discovery**
    - The server scans for connected devices or emulators via ports `5555–5585`.
3. **Emulator Port Pairing**
    - Each emulator has a unique pair of ports:
        - Even port → console connection (e.g., `5554`)
        - Odd port → ADB connection (e.g., `5555`)
    
```bash
Emulator 1: console = 5554, adb = 5555
Emulator 2: console = 5556, adb = 5557
```
    
4. **Accessing Devices**
    - Once connected, ADB commands can be issued to interact with devices.

---

## Installing ADB

### Debian-based Linux

```bash
sudo apt-get install adb
```

### Windows (Scoop)

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
iex (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
scoop bucket add extras
scoop install adb
```

Alternatively, install via Android Studio SDK:
```bash
C:\Users\<username>\AppData\Local\Android\Sdk\platform-tools\
```

Make sure to add this directory to your system’s `PATH` variable for easy access.

---

## Starting the ADB Server

```bash
adb start-server
```

Once the emulator or physical device is connected, you're ready to use ADB.

---

## Useful ADB Commands

|Command|Description|
|---|---|
|`adb help`|Display all available commands.|
|`adb kill-server`|Stop the ADB server.|
|`adb devices`|List connected devices.|
|`adb root`|Restart adbd with root permissions.|
|`adb install <apk>`|Install APK on the device.|
|`adb push <local> <remote>`|Transfer files to the device.|
|`adb pull <remote> <local>`|Retrieve files from the device.|
|`adb logcat`|View real-time device logs.|

---

## Example Usage

### List Connected Devices

```bash
adb devices
```

**Output:**

```bash
List of devices attached emulator-5554	device
```

### Execute Command on Device

```bash
adb shell whoami
```

**Output:**

```bash
shell
```

### Start Shell in Emulator

```bash
adb shell
```

**Output:**

```bash
emu64x:/ $
```

From here, Linux commands can be executed inside the Android environment.

---

## Android Shell Example

```bash
ls -l /sdcard/
```

**Sample Output:**

```bash
drwxrws--- 2 u0_a146  media_rw 4096 Alarms
drwxrws--x 5 media_rw media_rw 4096 Android
drwxrws--- 2 u0_a146  media_rw 4096 Audiobooks
...
```

---

## Gaining Root Access

ADB can provide elevated privileges on rooted devices or emulators using:
```bash
adb root
```

This restarts `adbd` with root permissions, allowing access to system directories and privileged operations.
