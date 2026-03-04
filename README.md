# Device Management Frontend

## 1. Giới thiệu
Device Management Frontend là giao diện người dùng (Dashboard) phục vụ cho việc hiển thị và giám sát các chỉ số hệ thống, thiết bị theo thời gian thực. Ứng dụng kết nối trực tiếp với Device Management Backend để hiển thị đồ thị, số liệu và cập nhật thông báo về tình trạng thiết bị.

## 2. Tính năng chính
- **Màn hình Dashboard trực quan**: Hiển thị tổng quan các thành phần hệ thống bao gồm thông tin CPU, biểu đồ truy xuất thông số hệ thống, mức RAM đang được sử dụng.
- **Tiếp nhận luồng dữ liệu thời gian thực (SSE)**: Kết nối và xử lý luồng sự kiện (Server-Sent Events) từ Backend để tự động cập nhật số liệu không cần tải lại trang.
- **Giao diện thân thiện và đáp ứng (Responsive)**: Hỗ trợ hiển thị trên nhiều cấu hình màn hình khác nhau.

## 3. Công nghệ sử dụng
- **Ngôn ngữ**: TypeScript, JavaScript
- **Framework & Libraries**: Next.js (App Router), React
- **Styling**: Tailwind CSS, Radix UI (shadcn/ui), Lucide React (Ícons)
- **Data Visualization**: Recharts (vẽ biểu đồ)
- **Package Manager**: pnpm

## 4. Hướng dẫn cài đặt và chạy dự án

### Yêu cầu môi trường
- Node.js (v18 trở lên)
- pnpm (Khuyến nghị sử dụng pnpm do dự án cấu hình bằng thẻ khóa `pnpm-lock.yaml`)

### Các bước cài đặt
1. Mở terminal và chuyển đến thư mục `device-management-ui`.
2. Cài đặt các thư viện phụ thuộc:
   ```bash
   pnpm install
   ```
3. Cấu hình biến môi trường: Đảm bảo kiểm tra file `.env.local` nếu có chỉ định host/port của API Backend.
4. Chạy ứng dụng web:
   ```bash
   pnpm run dev
   ```
5. Truy cập Dashboard tại địa chỉ `http://localhost:3000`.

## 5. Kiểm thử tự động
*Lưu ý: Mọi phương pháp kiểm thử chức năng Dashboard/UI đang được gom chung vào repo kiểm thử tự động `https://github.com/NguyenHai2003/playwright-test-device-management/blob/c0ee61e953652620992cec6b917ca07428cbf9a7/README.md` nhằm đảm bảo nhất quán end-to-end.*

## 6. Triển khai (Deployment lên AWS EC2)
Ứng dụng áp dụng quy trình Multi-stage Build và sử dụng NGINX để đóng gói Static Web giúp tăng cường khả năng phục vụ file frontend.

### Bước 1: Build Docker Image
Trên máy chủ triển khai hoặc EC2:
```bash
docker build -t device-management-ui .
```

### Bước 2: Chạy Docker Container
Sau khi build xong, khởi chạy Container với cấu hình mapping với NGINX (phục vụ qua port 80):
```bash
docker run -d -p 80:80 --name dev-management-dashboard device-management-ui
```
Hãy chắc chắn rằng Firewall / AWS Security Group đã cho phép (Allow) kết nối Inbound tới Port `80` (HTTP) hoặc `443` (HTTPS) trên máy chủ EC2.
