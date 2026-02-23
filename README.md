Eldercare App – Hệ Thống Giám Sát Sức Khỏe Người Cao Tuổi
📌 Giới thiệu

Eldercare App là hệ thống giám sát sức khỏe từ xa sử dụng thiết bị ESP32 kết hợp với ứng dụng Flutter và Backend FastAPI.
Hệ thống cho phép theo dõi:

❤️ Nhịp tim (Heart Rate)

🩸 Nồng độ SpO2

🌡 Nhiệt độ cơ thể

📈 ECG theo yêu cầu

🚨 Cảnh báo vượt ngưỡng

Dữ liệu được lưu trữ trên MongoDB và truy cập thông qua API bảo mật.

🏗 Kiến trúc hệ thống
ESP32
   ↓ HTTPS (X-Device-Token)
Cloudflare
   ↓ Tunnel (cloudflared)
FastAPI (Backend)
   ↓
MongoDB

Flutter App
   ↓ HTTPS (X-API-Key)
FastAPI
⚙ Công nghệ sử dụng
📱 Frontend

Flutter

Provider (State Management)

HTTP

🌐 Backend

FastAPI

Python

Pydantic

Uvicorn

🗄 Database

MongoDB

🔐 Bảo mật

X-Device-Token (cho ESP32)

X-API-Key (cho Flutter)

Cloudflare Tunnel

📂 Cấu trúc Backend
backend/
 ├── main.py
 ├── db.py
 ├── api/
 │    ├── esp.py
 │    ├── devices.py
 │    ├── health.py
 │    └── users.py
 ├── services/
 │    └── health_service.py
 ├── utils/
 │    └── auth.py
 └── models/
🔄 Luồng hoạt động hệ thống
1️⃣ Luồng gửi dữ liệu sức khỏe (Vitals)
ESP32 gửi dữ liệu
POST /api/v1/esp/devices/{device_id}/readings
Header: X-Device-Token

Backend:

Kiểm tra token

Validate dữ liệu

Lưu MongoDB (health_readings)

Kiểm tra ngưỡng → tạo alert nếu cần

Flutter lấy dữ liệu
GET /api/v1/users/{user_id}/latest
GET /api/v1/users/{user_id}/history
2️⃣ Luồng đo ECG theo yêu cầu
Bước 1 – Flutter yêu cầu đo ECG
POST /api/v1/devices/{device_id}/ecg/request
Header: X-API-Key

Backend tạo:

device_commands
status = pending
Bước 2 – ESP hỏi lệnh
GET /api/v1/esp/devices/{device_id}/commands/next

Backend trả:

ecg_request
Bước 3 – ESP gửi dữ liệu ECG
POST /api/v1/esp/devices/{device_id}/readings
Bước 4 – ESP xác nhận hoàn thành
POST /api/v1/esp/devices/{device_id}/commands/{command_id}/ack
🗄 Cấu trúc Database
health_readings

device_id

heart_rate

spo2

temperature

ecg

timestamp

device_commands

device_id

type

status (pending, dispatched, done)

created_at

devices

device_id

device_token

user_id

users

user_id

name

threshold_settings

alerts

user_id

type

value

created_at

🔐 Cơ chế bảo mật
Đối tượng	Header
ESP32	X-Device-Token
Flutter	X-API-Key

Tách quyền thiết bị và người dùng

Không cho phép truy cập trái phép

Dữ liệu truyền qua HTTPS

🚀 Hướng dẫn chạy Backend
1️⃣ Cài đặt thư viện
pip install -r requirements.txt
2️⃣ Chạy server
uvicorn main:app --reload

Server chạy tại:

http://localhost:8000
