# 📱 Eldercare App  
Hệ thống giám sát sức khỏe người cao tuổi sử dụng ESP32 + FastAPI + MongoDB

---

## 🚨 Tính năng chính

- ❤️ Theo dõi nhịp tim (Heart Rate)
- 🩸 Theo dõi SpO2
- 🌡 Theo dõi nhiệt độ
- 📈 Đo ECG theo yêu cầu
- 🚨 Cảnh báo vượt ngưỡng

Dữ liệu được lưu trữ trên MongoDB và truy cập thông qua API bảo mật.

---

## 🏗 Kiến trúc hệ thống

### 🔌 Thiết bị ESP32

ESP32  
⬇ HTTPS (X-Device-Token)  
⬇ Cloudflare  
⬇ Tunnel (cloudflared)  
⬇ FastAPI (Backend)  
⬇ MongoDB  

---

### 📱 Flutter App

Flutter App  
⬇ HTTPS (X-API-Key)  
⬇ FastAPI  
⬇ MongoDB  

---

## 🛠 Công nghệ sử dụng

### 📱 Frontend
- Flutter
- Provider (State Management)
- HTTP

### 🌐 Backend
- FastAPI
- Python
- Pydantic
- Uvicorn

### 🗄 Database
- MongoDB

### 🔐 Bảo mật
- X-Device-Token (ESP32)
- X-API-Key (Flutter)
- Cloudflare Tunnel

---

## 🔄 Luồng hoạt động

### 1️⃣ Gửi dữ liệu sức khỏe

ESP32 gửi:
```bash
POST /api/v1/esp/devices/{device_id}/readings
Flutter lấy dữ liệu:
GET /api/v1/users/{user_id}/latest2️⃣ Đo ECG theo yêu cầu

Flutter gửi yêu cầu:

POST /api/v1/devices/{device_id}/ecg/request

ESP nhận lệnh:

GET /api/v1/esp/devices/{device_id}/commands/next

ESP gửi dữ liệu ECG:

POST /api/v1/esp/devices/{device_id}/readings
🚀 Hướng dẫn chạy
Backend
pip install -r requirements.txt
uvicorn main:app --reload
Flutter
flutter pub get
flutter run
