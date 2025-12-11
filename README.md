# Sora Video Downloader – Công cụ lấy link video không watermark

<img width="867" height="537" alt="UI" src="https://github.com/user-attachments/assets/458b4132-f26e-4fa5-a87b-1c26890403fc" />

## ✨ Tính năng

- **Dễ sử dụng** – Chỉ cần dán link Sora để lấy URL tải trực tiếp.
- **Video không watermark** – Trích xuất link gốc từ trường `encodings.source.path`.
- **Ổn định lâu dài** – Tích hợp cơ chế tự làm mới `access_token`.
- **Triển khai qua Docker** – Build và chạy chỉ với một lệnh.
- **Quản lý cấu hình qua `.env`** – Bảo mật và hỗ trợ reload.
- **Hỗ trợ proxy** – Cấu hình bằng biến môi trường `HTTP_PROXY`.
- **Bảo vệ truy cập (tùy chọn)** – Dùng `APP_ACCESS_TOKEN` để khóa giao diện web.

## 🛠️ Công nghệ sử dụng

- **Backend:** Python, Flask, Gunicorn
- **HTTP Client:** `curl-cffi`
- **Frontend:** HTML, CSS, JavaScript thuần
- **Config:** `python-dotenv`
- **Triển khai:** Docker

## 🚀 Bắt đầu nhanh

### 1. Yêu cầu

- Docker hoặc Python

## 2. Lấy thông tin xác thực OpenAI

Bạn cần lấy **SORA_AUTH_TOKEN** hoặc **SORA_REFRESH_TOKEN**.

### 📌 Phương pháp 1: Android (Root) + bắt gói tin

#### Công cụ cần có

- Android Root (Magisk)
- LSPosed
- Reqable (PC)
- TrustMeAlready module

#### Các bước

1. Chuẩn bị thiết bị Android và vượt SafetyNet/Play Integrity nếu cần.
2. Cài TrustMeAlready và kích hoạt cho ứng dụng Sora.
3. Cấu hình Reqable + proxy (nếu cần), cài chứng chỉ.
4. Bắt request POST đến `auth.openai.com/oauth/token` và lưu:
   - `client_id`
   - `refresh_token`

### 📌 Phương pháp 2: iOS (Jailbreak)

Tham khảo:  
https://github.com/qy527145/devicecheck

## 3. Tải và cấu hình dự án

```bash
git clone https://github.com/tibbar213/sora-downloader.git
cd sora-downloader
cp .env.example .env
```

Điền token vào `.env`:

```ini
SORA_AUTH_TOKEN="access_token"
SORA_REFRESH_TOKEN="refresh_token"
SORA_CLIENT_ID="client_id"

APP_ACCESS_TOKEN="mật_khẩu_tùy_chọn"
HTTP_PROXY="http://127.0.0.1:7890"
```

## 4. Build và chạy Docker

### Build

```bash
docker build -t sora-downloader .
```

### Chạy container

```bash
docker run -d -p 5000:8000   -v $(pwd)/.env:/app/.env   --name sora-downloader   sora-downloader
```

Windows PowerShell:

```
-v ${PWD}/.env:/app/.env
```

Windows CMD:

```
-v %cd%\.env:/app/.env
```

## 5. Truy cập giao diện

```
http://localhost:5000
```

---

# ⚙️ Cấu hình `.env`

| Biến | Bắt buộc | Mô tả |
|------|----------|--------|
| `SORA_AUTH_TOKEN` | ✔ | Token dùng gọi API Sora |
| `SORA_REFRESH_TOKEN` | Khuyến nghị | Làm mới access token |
| `SORA_CLIENT_ID` | Khuyến nghị | Dùng khi refresh token |
| `APP_ACCESS_TOKEN` | Không | Khóa giao diện web |
| `HTTP_PROXY` | Không | Proxy HTTP/S |

---

## 🌟 Dự án đề xuất

- **sora2api**: https://github.com/TheSmallHanCat/sora2api

## 📄 Miễn trừ trách nhiệm

- Chỉ phục vụ mục đích nghiên cứu.
- Tuân thủ điều khoản OpenAI.
- Người dùng tự chịu trách nhiệm khi sử dụng.
