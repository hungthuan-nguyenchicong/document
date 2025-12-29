# A. Cấu trúc Thư mục (Folder Structure)
src/
├── api/              # Axios instance, định nghĩa các hàm gọi Laravel
├── components/       # Các UI dùng chung (Button, Input, Layout)
├── middlewares/      # Các file như authMiddleware.jsx
├── routes/           # Cấu hình routes.jsx tập trung
├── pages/            # Mỗi page gồm: Component, Loader, Action
│   ├── Product/
│   │   ├── ProductList.jsx
│   │   ├── ProductLoader.js
│   │   └── ProductAction.js
└── store/            # Quản lý state toàn cục (Zustand hoặc Context)

# Các đầu mục Logic quan trọng (Base Logic)
Axios Interceptor: * Tự động đính Authorization: Bearer <token> vào mọi request.

Xử lý lỗi 401 (Unauthorized) tập trung: tự động xóa token và đẩy người dùng về trang Login.

Auth Provider / Context: Lưu trữ thông tin User hiện tại sau khi Login thành công để các Component khác sử dụng.

Global Error Element: Một Component trang 404 hoặc "Something went wrong" để gắn vào root route.

# 3. Sơ đồ vận hành Base Project
Dưới đây là cách mà Base Project của bạn sẽ xử lý một luồng CRUD chuẩn:

Request: Trình duyệt gọi URL.

Middleware: Kiểm tra localStorage tìm Token. Nếu không có, throw redirect('/login').

Loader: Gọi API Laravel (đã có Axios Interceptor lo phần đính Token).

Component: Hiển thị dữ liệu. Nếu bấm "Lưu", gọi Action.

Action: Gửi dữ liệu qua Laravel. Nếu Laravel báo lỗi Validation (422), Action trả lỗi về cho Component hiển thị.

# 4. Danh sách các tính năng "phải có" trong Base Project
Tính năng	Mô tả
Login/Logout	Luồng đầy đủ từ lưu Token đến xóa sạch bộ nhớ khi thoát.
Flash Messages	Thông báo (Toast) hiện lên sau khi Action thực hiện thành công (ví dụ: "Đã xóa sản phẩm").
Validation Handling	Tự động ánh xạ lỗi từ Laravel (Array errors) lên các ô Input trong React.
Loading Progress	Thanh tiến trình (nprogress) chạy trên đầu trang mỗi khi Loader hoặc Action đang thực thi.