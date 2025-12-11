

Sora – Công cụ lấy link video không watermark (Sora Video Downloader Web UI)

<img width="867" height="537" alt="image" src="https://github.com/user-attachments/assets/458b4132-f26e-4fa5-a87b-1c26890403fc" />



⸻

✨ Tính năng nổi bật
	•	Dễ sử dụng: Chỉ cần dán link Sora vào là có ngay link tải trực tiếp.
	•	Video không watermark: Công cụ sẽ trích xuất link gốc nằm trong encodings.source.path.
	•	Chạy ổn định lâu dài: Tích hợp cơ chế tự làm mới access_token, không cần làm thủ công.
	•	Hỗ trợ Docker: Một lệnh build & chạy, không lo môi trường.
	•	Quản lý cấu hình qua environment: Tất cả thông tin nhạy cảm quản lý bằng file .env, bảo mật – tiện dụng – hỗ trợ reload.
	•	Có hỗ trợ proxy: Dùng HTTP_PROXY để định tuyến request OpenAI API qua proxy.
	•	Hỗ trợ đặt mật khẩu truy cập: Dùng APP_ACCESS_TOKEN để khóa trang web, tránh bị spam/abuse.

⸻

🛠️ Tech stack
	•	Backend: Python, Flask, Gunicorn
	•	HTTP requests: curl-cffi (mô phỏng TLS/JA3 giống trình duyệt → tăng tỉ lệ request thành công)
	•	Frontend: HTML, CSS, JavaScript thuần
	•	Quản lý cấu hình: python-dotenv
	•	Triển khai: Docker

⸻

🚀 Bắt đầu nhanh

1. Yêu cầu trước khi dùng
	•	Máy đã cài Docker hoặc Python.

2. Lấy token xác thực OpenAI

Đây là bước quan trọng nhất. Bạn cần lấy:
	•	SORA_AUTH_TOKEN (hiệu lực ngắn hạn)
	•	hoặc SORA_REFRESH_TOKEN (hiệu lực dài hạn)

⸻

🔹 Cách 1 (khuyến nghị): Android (Root) + bắt gói tin

Phương pháp này cần thiết bị Android đã root, và có chút kỹ năng thao tác.

Ý tưởng chính:
Dùng môi trường Root + mô-đun bypass SSL Pinning của ứng dụng Sora → bắt request đăng nhập → lấy token xác thực.

Chuẩn bị:
	•	Android đã Root (Magisk).
	•	LSPosed (cài qua Magisk).
	•	Công cụ bắt gói tin Reqable (khuyến nghị dùng trên PC).
	•	Module bypass SSL Pinning: TrustMeAlready (dành cho LSPosed).

⸻

Các bước thực hiện

1. Chuẩn bị môi trường Android
	•	Thiết bị phải Root + cài LSPosed.
	•	Để chạy được app Sora trong môi trường Root, bạn có thể phải vượt qua SafetyNet / Play Integrity.
→ Tham khảo bài này trên Coolapk:
Hướng dẫn Google 3 xanh rất dễ￼

2. Cài module bypass
	•	Mở LSPosed Manager → cài TrustMeAlready
	•	Bật module và tick vào ứng dụng Sora
	•	Khởi động lại thiết bị

3. Cấu hình Reqable
	•	Cài Reqable trên máy tính và chạy
	•	Nếu máy tính dùng proxy (Clash, V2RayN…), cần:
	•	Bật Allow LAN
	•	Trong Reqable → đặt Upstream Proxy trỏ đến proxy cục bộ:
http://127.0.0.1:7890
	•	Đảm bảo PC & điện thoại cùng WiFi
	•	Cài chứng chỉ Reqable:
	•	Điện thoại truy cập reqable.pro/ssl
	•	Vì máy đã Root → nên cài ở dạng system certificate

4. Bắt token
	•	Bật bắt gói trong Reqable
	•	Mở app Sora trên điện thoại → đăng nhập
	•	Tìm request POST đến:
auth.openai.com/oauth/token
	•	Trong response sẽ có:
	•	client_id → ghi vào SORA_CLIENT_ID
	•	refresh_token → ghi vào SORA_REFRESH_TOKEN

⚠️ Lưu ý quan trọng:
	•	Refresh token lâu hết hạn nhưng sẽ thay đổi sau mỗi lần làm mới → lưu cẩn thận.
	•	Thao tác root & sửa hệ thống tiềm ẩn rủi ro → cân nhắc kỹ.
	•	Token là thông tin nhạy cảm – không chia sẻ cho ai.

⸻

🔹 Cách 2: iOS (Jailbreak)

Cần máy iOS đã jailbreak.

Tham khảo:
	•	https://github.com/qy527145/devicecheck

Tác giả không có thiết bị iOS nên chưa kiểm tra độ tương thích — welcome PR.

⸻

3. Tải và cấu hình dự án
	1.	Clone dự án:

git clone https://github.com/tibbar213/sora-downloader.git
cd sora-downloader

	2.	Tạo file .env:

cp .env.example .env

	3.	Điền token vào .env:

SORA_AUTH_TOKEN="access_token của bạn"
SORA_REFRESH_TOKEN="refresh_token"
SORA_CLIENT_ID="client_id"

APP_ACCESS_TOKEN="mật khẩu truy cập trang (tùy chọn)"

HTTP_PROXY="http://địa_chỉ_proxy:port" 


⸻

4. Build & chạy bằng Docker

Trong thư mục dự án:

Build:

docker build -t sora-downloader .

Chạy:

docker run -d -p 5000:8000 \
  -v $(pwd)/.env:/app/.env \
  --name sora-downloader \
  sora-downloader

Giải thích:
	•	-d → chạy nền
	•	-p 5000:8000 → map port host → container
	•	-v .env:/app/.env → bắt buộc để token refresh tự lưu lại
	•	--name → đặt tên cho dễ quản lý

Windows PowerShell:

-v ${PWD}/.env:/app/.env

Windows CMD:

-v %cd%\\.env:/app/.env


⸻

5. Truy cập giao diện web

Mở trình duyệt:

👉 http://localhost:5000
hoặc IP server tương ứng.

⸻

⚙️ Cấu hình .env

Biến	Bắt buộc?	Mô tả
SORA_AUTH_TOKEN	Có	Access Token để gọi API Sora. Nếu để trống và có refresh token → hệ thống tự lấy mới.
SORA_REFRESH_TOKEN	Không (nhưng nên có)	Dùng để làm mới access token khi hết hạn.
SORA_CLIENT_ID	Không (nhưng cần nếu dùng refresh)	Lấy từ request đăng nhập khi bắt gói.
APP_ACCESS_TOKEN	Không	Mật khẩu bảo vệ trang web.
HTTP_PROXY	Không	Proxy HTTP(S) nếu server bị chặn mạng.


⸻

🌟 Gợi ý nên dùng kèm
	•	sora2api – API Sora miễn phí/phi chính thức
https://github.com/TheSmallHanCat/sora2api
→ Dự án này tương thích hoàn toàn, có mục “custom parser URL”.

⸻

📄 Cam kết & Miễn trừ trách nhiệm
	•	Dự án chỉ dành cho mục đích nghiên cứu kỹ thuật.
	•	Hãy tuân thủ điều khoản OpenAI.
	•	Bạn tự chịu trách nhiệm về mọi hậu quả khi sử dụng công cụ.

⸻
