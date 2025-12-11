# Android ADB Commands

ADB (Android Debug Bridge) là công cụ dòng lệnh để giao tiếp với thiết bị Android.

## Cài đặt

```bash
# Windows (với Chocolatey)
choco install adb

# macOS
brew install android-platform-tools

# Linux (Ubuntu/Debian)
sudo apt install adb
```

## Kết nối thiết bị

### USB
```bash
# Kiểm tra thiết bị đã kết nối
adb devices

# Nếu có nhiều thiết bị, chỉ định serial
adb -s <serial> <command>
```

### Wireless (WiFi)
```bash
# Bước 1: Kết nối USB trước, bật TCP/IP
adb tcpip 5555

# Bước 2: Lấy IP thiết bị (Settings > About > IP Address)
adb connect 192.168.1.100:5555

# Ngắt kết nối
adb disconnect 192.168.1.100:5555
```

---

## Quản lý ứng dụng

### Cài đặt APK
```bash
# Cài đặt
adb install app.apk

# Cài đặt ghi đè (update)
adb install -r app.apk

# Cài đặt cho tất cả users
adb install -g app.apk

# Cài đặt test APK
adb install -t app.apk
```

### Gỡ cài đặt
```bash
# Gỡ hoàn toàn
adb uninstall com.example.app

# Giữ data
adb uninstall -k com.example.app
```

### Liệt kê packages
```bash
# Tất cả packages
adb shell pm list packages

# Chỉ system apps
adb shell pm list packages -s

# Chỉ 3rd party apps
adb shell pm list packages -3

# Tìm kiếm
adb shell pm list packages | grep "keyword"
```

### Quản lý app
```bash
# Xóa data app
adb shell pm clear com.example.app

# Disable app
adb shell pm disable-user com.example.app

# Enable app
adb shell pm enable com.example.app

# Force stop
adb shell am force-stop com.example.app
```

---

## Truyền file

```bash
# Copy file từ PC vào thiết bị
adb push local_file.txt /sdcard/

# Copy file từ thiết bị ra PC
adb pull /sdcard/file.txt ./

# Copy folder
adb push ./folder /sdcard/
adb pull /sdcard/folder ./
```

---

## Shell Commands

### Truy cập shell
```bash
# Interactive shell
adb shell

# Chạy lệnh đơn
adb shell <command>

# Shell với quyền root (nếu có)
adb shell su
```

### Thao tác file
```bash
# Liệt kê file
adb shell ls /sdcard/

# Xem nội dung file
adb shell cat /sdcard/file.txt

# Tạo folder
adb shell mkdir /sdcard/newfolder

# Xóa file/folder
adb shell rm /sdcard/file.txt
adb shell rm -rf /sdcard/folder

# Copy/Move
adb shell cp /sdcard/a.txt /sdcard/b.txt
adb shell mv /sdcard/a.txt /sdcard/b.txt
```

---

## Screenshot & Record

```bash
# Chụp màn hình
adb shell screencap /sdcard/screenshot.png
adb pull /sdcard/screenshot.png

# Quay video (tối đa 3 phút)
adb shell screenrecord /sdcard/video.mp4

# Quay với options
adb shell screenrecord --time-limit 60 --bit-rate 6000000 /sdcard/video.mp4

# Dừng quay: Ctrl+C
```

---

## Logcat

```bash
# Xem tất cả log
adb logcat

# Clear log buffer
adb logcat -c

# Filter theo tag
adb logcat -s "MyApp"

# Filter theo priority (V/D/I/W/E/F)
adb logcat *:E  # Chỉ Error trở lên

# Filter theo package
adb logcat --pid=$(adb shell pidof com.example.app)

# Lưu log ra file
adb logcat > log.txt

# Format log
adb logcat -v time      # Thêm timestamp
adb logcat -v threadtime # Thêm thread + timestamp
```

---

## Thông tin thiết bị

```bash
# Thông tin chung
adb shell getprop

# Android version
adb shell getprop ro.build.version.release

# SDK version
adb shell getprop ro.build.version.sdk

# Model
adb shell getprop ro.product.model

# Serial number
adb shell getprop ro.serialno

# Độ phân giải màn hình
adb shell wm size

# Density
adb shell wm density

# Battery info
adb shell dumpsys battery
```

---

## Input Events

### Touch & Swipe
```bash
# Tap tại tọa độ (x, y)
adb shell input tap 500 500

# Swipe từ (x1,y1) đến (x2,y2) trong ms
adb shell input swipe 100 500 100 100 300

# Long press
adb shell input swipe 500 500 500 500 1000
```

### Keyevent
```bash
# Các phím thông dụng
adb shell input keyevent KEYCODE_HOME        # 3
adb shell input keyevent KEYCODE_BACK        # 4
adb shell input keyevent KEYCODE_MENU        # 82
adb shell input keyevent KEYCODE_POWER       # 26
adb shell input keyevent KEYCODE_VOLUME_UP   # 24
adb shell input keyevent KEYCODE_VOLUME_DOWN # 25
adb shell input keyevent KEYCODE_ENTER       # 66
adb shell input keyevent KEYCODE_DEL         # 67

# Mở camera
adb shell input keyevent KEYCODE_CAMERA      # 27
```

### Text input
```bash
# Nhập text (không hỗ trợ tiếng Việt)
adb shell input text "hello"

# Với khoảng trắng, dùng %s
adb shell input text "hello%sworld"
```

---

## Activity Manager

```bash
# Mở app
adb shell am start -n com.example.app/.MainActivity

# Mở URL
adb shell am start -a android.intent.action.VIEW -d "https://google.com"

# Gọi điện
adb shell am start -a android.intent.action.CALL -d tel:0123456789

# Gửi broadcast
adb shell am broadcast -a android.intent.action.BOOT_COMPLETED

# Kill app
adb shell am force-stop com.example.app
```

---

## Network

```bash
# Bật/tắt WiFi
adb shell svc wifi enable
adb shell svc wifi disable

# Bật/tắt Mobile Data
adb shell svc data enable
adb shell svc data disable

# Xem IP
adb shell ip addr show wlan0

# Xem kết nối mạng
adb shell netstat
```

---

## Settings

```bash
# Lấy setting
adb shell settings get system screen_brightness
adb shell settings get global airplane_mode_on

# Set setting
adb shell settings put system screen_brightness 255
adb shell settings put global airplane_mode_on 1

# Các namespace: system, secure, global
```

---

## Backup & Restore

```bash
# Backup toàn bộ
adb backup -all -f backup.ab

# Backup 1 app (bao gồm data)
adb backup -f app_backup.ab com.example.app

# Restore
adb restore backup.ab
```

---

## Scrcpy (Bonus)

[Scrcpy](https://github.com/Genymobile/scrcpy) - Mirror màn hình Android lên PC qua ADB.

```bash
# Cài đặt
# Windows: choco install scrcpy
# macOS: brew install scrcpy
# Linux: apt install scrcpy

# Chạy
scrcpy

# Với options
scrcpy --max-size 1024
scrcpy --bit-rate 2M
scrcpy --record file.mp4
```

---

## Troubleshooting

### Device not found
```bash
# Restart ADB server
adb kill-server
adb start-server
```

### Unauthorized
- Kiểm tra popup "Allow USB debugging" trên thiết bị
- Revoke USB debugging authorizations trong Developer Options rồi kết nối lại

### Multiple devices
```bash
# Liệt kê devices
adb devices

# Chọn device cụ thể
adb -s SERIAL_NUMBER shell
```
