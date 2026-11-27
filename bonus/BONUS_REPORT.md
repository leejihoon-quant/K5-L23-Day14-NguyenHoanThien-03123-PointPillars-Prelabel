# Báo cáo Phần Điểm Cộng — Hướng 2: Model 3D (PointPillars) trong CVAT (Serverless / Nuclio)

- **Mã học viên:** `123` (`2A202603123` — Nguyễn Hoàn Thiện)
- **Task mục tiêu:** `D14-123-B` (`http://localhost:8080/tasks/46`)

---

## 1. Các bước đã thực hiện
1. Kiểm tra tài liệu hướng dẫn `bonus/huong-dan-model-3d.html` và `bonus/README.md`.
2. Kiểm tra dịch vụ Nuclio tại `http://localhost:8070` và endpoint serverless của CVAT tại `GET http://localhost:8080/api/lambda/functions` (với token đăng nhập CVAT).
3. Kiểm tra quy trình gọi API cho task 3D (`POST /api/lambda/requests`) khi giao diện Automatic Annotation của CVAT báo `"No models available"` cho dữ liệu 3D.

---

## 2. Vướng mắc hiện tại & Log lỗi thực tế
Container `nuclio` chưa được khởi chạy kèm trong cụm Docker CVAT hiện tại (do CVAT được bật mặc định không kèm `components/serverless/docker-compose.serverless.yml` và hết thời gian triển khai trước hạn 12:00):

- **Kiểm tra cổng Nuclio (`http://localhost:8070`):**
  ```text
  <urlopen error [WinError 10061] No connection could be made because the target machine actively refused it>
  ```
- **Kiểm tra API CVAT (`GET http://localhost:8080/api/lambda/functions`):**
  ```text
  HTTP 503 Service Unavailable
  "HTTPConnectionPool(host='nuclio', port=8070): Max retries exceeded with url: /api/functions
  (Caused by NameResolutionError(\"HTTPConnection(host='nuclio', port=8070): Failed to resolve 'nuclio' ([Errno -5] No address associated with hostname)\"))"
  ```

---

## 3. Nếu làm tiếp sẽ thử gì
1. Khởi động lại cụm Docker CVAT kèm file serverless và bind dashboard Nuclio về `127.0.0.1:8070`:
   ```powershell
   docker compose -f docker-compose.yml -f components/serverless/docker-compose.serverless.yml up -d
   ```
2. Đóng gói hàm PointPillars (dùng `vf_pp` bản CPU) thành serverless function (`function.yaml` + `main.py` đọc file `.pcd` trong frame và trả về danh sách `cuboid` dạng `shape`) rồi deploy lên Nuclio bằng `nuctl deploy`.
3. Gọi API `POST /api/lambda/requests {"function": "<tên>", "task": 46, "threshold": 0.5}` để CVAT tự sinh cuboid 3D, sau đó xuất `ref_B` đối chiếu số box trùng tâm trong `0,5 m` với `d14_123/pred_B.json`.
