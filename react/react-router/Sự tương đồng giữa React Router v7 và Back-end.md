# Sự tương đồng giữa React Router v7 và Back-end
Nếu bạn đã từng làm việc với Laravel, Express hay ASP.NET, bạn sẽ thấy mô hình của React Router v7 giống hệt:

Thành phần React RouterTương đương trong Back-endChức năngroutes.jsxRoute File (web.php, routes/index.js)

Định nghĩa sơ đồ điều hướng của toàn bộ hệ thống.

middlewareHTTP MiddlewareKiểm tra quyền (Auth), Logging, xử lý request trước khi vào Controller.loader

Controller (Action GET)Chuẩn bị dữ liệu từ Database/API trước khi trả về View.actionController (Action POST/PUT)Xử lý dữ liệu từ Form gửi lên (Mutation).ComponentView / Template (Blade, EJS)Nhận dữ liệu và render ra HTML cho người dùng.

## note
routes.jsx => Route File (web.php, routes/index.js)

middleware => HTTP Middleware

loader => Controller (Action GET)

action => Controller (Action POST/PUT)

Component => View / Template (Blade, EJS)