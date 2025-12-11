
Sora Video Downloader – Công cụ lấy link video không watermark

<img width="867" height="537" alt="UI" src="https://github.com/user-attachments/assets/458b4132-f26e-4fa5-a87b-1c26890403fc" />



⸻

✨ Tính năng
	•	Dễ sử dụng – Chỉ cần dán link Sora để lấy URL tải trực tiếp.
	•	Video không watermark – Trích xuất link gốc từ trường encodings.source.path.
	•	Ổn định lâu dài – Tích hợp cơ chế tự làm mới access_token.
	•	Triển khai qua Docker – Build và chạy chỉ với một lệnh.
	•	Quản lý cấu hình qua .env – Bảo mật và hỗ trợ reload.
	•	Hỗ trợ proxy – Cấu hình bằng biến môi trường HTTP_PROXY.
	•	Bảo vệ truy cập (tùy chọn) – Dùng APP_ACCESS_TOKEN để khóa giao diện web.

⸻

🛠️ Công nghệ sử dụng
	•	Backend: Python, Flask, Gunicorn
	•	HTTP Client: curl-cffi (mô phỏng TLS/JA3 giống trình duyệt → tăng tỷ lệ request thành công)
	•	Frontend: HTML, CSS, JavaScript thuần
	•	Config: python-dotenv
	•	Triển khai: Docker

⸻

🚀 Bắt đầu nhanh

1. Yêu cầu
	•	Docker hoặc Python

⸻

2. Lấy thông tin xác thực OpenAI

Bạn cần lấy SORA_AUTH_TOKEN (ngắn hạn) hoặc SORA_REFRESH_TOKEN (dài hạn).

📌 Phương pháp 1 (Khuyến nghị): Android (Root) + bắt gói tin

Mục đích: Bypass SSL Pinning của ứng dụng Sora → bắt request đăng nhập → trích xuất token.

Công cụ cần có
	•	Thiết bị Android đã Root (Magisk)
	•	LSPosed
	•	Công cụ bắt gói tin Reqable (khuyến nghị chạy trên PC)
	•	Module bypass SSL Pinning: TrustMeAlready

Các bước thực hiện
	1.	Chuẩn bị thiết bị Android
	•	Root + cài LSPosed
	•	Nếu Sora không chạy do SafetyNet/Play Integrity → xem hướng dẫn:
	•	https://www.coolapk.com/feed/68354277?s=NGRlYjI5NjQxNmI5MDZnNjkwYjE5Yzl6a1571
	2.	Cài module bypass
	•	Mở LSPosed → cài TrustMeAlready
	•	Bật module và chọn áp dụng cho Sora App
	•	Khởi động lại máy
	3.	Cấu hình Reqable
	•	Cài Reqable trên PC và mở lên
	•	Nếu dùng proxy như Clash/V2Ray:
	•	Bật “Allow LAN”
	•	Trong Reqable đặt “Upstream Proxy” → http://127.0.0.1:7890
	•	Đảm bảo điện thoại và PC chung Wi-Fi
	•	Cài chứng chỉ Reqable:
	•	Truy cập reqable.pro/ssl
	•	Đối với máy đã root → nên cài dạng system certificate
	4.	Bắt token
	•	Bật bắt gói trong Reqable
	•	Mở Sora trên điện thoại → đăng nhập
	•	Tìm request POST đến:

auth.openai.com/oauth/token


	•	Trong response lấy:
	•	client_id → ghi vào SORA_CLIENT_ID
	•	refresh_token → ghi vào SORA_REFRESH_TOKEN

⚠️ Lưu ý
	•	Refresh token dài hạn nhưng sẽ thay đổi sau khi làm mới
	•	Token là thông tin cực kỳ nhạy cảm – không chia sẻ
	•	Root thiết bị có rủi ro, tự cân nhắc

⸻

📌 Phương pháp 2: iOS (Jailbreak)

Cần thiết bị iOS đã jailbreak.
Tham khảo:
	•	https://github.com/qy527145/devicecheck

Tác giả chưa có máy iOS để kiểm thử.

⸻

3. Tải và cấu hình dự án

Clone mã nguồn:

git clone https://github.com/tibbar213/sora-downloader.git
cd sora-downloader

Tạo file .env:

cp .env.example .env

Điền token vào .env:

# --- OpenAI Sora API ---
SORA_AUTH_TOKEN="access_token"
SORA_REFRESH_TOKEN="refresh_token"
SORA_CLIENT_ID="client_id"

# --- Bảo vệ truy cập (tùy chọn) ---
APP_ACCESS_TOKEN="mật_khẩu_tùy_chọn"

# --- Proxy (tùy chọn) ---
HTTP_PROXY="http://127.0.0.1:7890"


⸻

4. Build và chạy Docker

Build

docker build -t sora-downloader .

Chạy container

docker run -d -p 5000:8000 \
  -v $(pwd)/.env:/app/.env \
  --name sora-downloader \
  sora-downloader

Giải thích:

Tham số	Ý nghĩa
-d	chạy nền
-p 5000:8000	map cổng host → container
-v .env:/app/.env	cho phép lưu lại token khi auto refresh
--name	đặt tên container

Windows PowerShell:

-v ${PWD}/.env:/app/.env

Windows CMD:

-v %cd%\\.env:/app/.env


⸻

5. Truy cập giao diện

http://localhost:5000

Hoặc IP server + port bạn đã cấu hình.

⸻

⚙️ Cấu hình .env

Biến	Bắt buộc	Mô tả
SORA_AUTH_TOKEN	✔	Token dùng để gọi API Sora
SORA_REFRESH_TOKEN	Khuyến nghị	Làm mới access token khi hết hạn
SORA_CLIENT_ID	Khuyến nghị	Lấy cùng refresh token khi bắt gói
APP_ACCESS_TOKEN	Không	Bảo vệ giao diện web
HTTP_PROXY	Không	Proxy HTTP/S cho server


⸻

🌟 Dự án đề xuất
	•	sora2api – Sora API phi chính thức, tương thích với dự án này
https://github.com/TheSmallHanCat/sora2api

⸻

📄 Miễn trừ trách nhiệm
	•	Dự án chỉ phục vụ mục đích nghiên cứu và học tập.
	•	Vui lòng tuân thủ điều khoản sử dụng của OpenAI.
	•	Người dùng tự chịu trách nhiệm cho mọi hậu quả khi sử dụng công cụ.

⸻
