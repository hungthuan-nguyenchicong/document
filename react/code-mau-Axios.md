# đoạn code mẫu cho Axios Interceptor chuẩn chỉnh

```bash
import axios from "axios";

const axiosClient = axios.create({
    baseURL: "http://localhost:8000/api", // URL của Laravel API
    headers: {
        "Content-Type": "application/json",
        "Accept": "application/json",
    },
});

// 1. Interceptor cho Request: Trước khi gửi request lên Laravel
axiosClient.interceptors.request.use((config) => {
    const token = localStorage.getItem("laravel_token");
    if (token) {
        config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
});

// 2. Interceptor cho Response: Sau khi nhận dữ liệu từ Laravel về
axiosClient.interceptors.response.use(
    (response) => {
        // Nếu thành công, chỉ trả về dữ liệu bên trong để rút gọn code ở Component
        return response.data;
    },
    (error) => {
        const { response } = error;

        if (response) {
            // Trường hợp 401: Token hết hạn hoặc không hợp lệ
            if (response.status === 401) {
                localStorage.removeItem("laravel_token");
                
                // Sử dụng window.location để force redirect về trang login
                // Vì lúc này ta có thể đang ở ngoài "thế giới" React Component
                window.location.href = "/login";
            }

            // Trường hợp 422: Lỗi Validation từ Laravel
            if (response.status === 422) {
                // Trả lỗi về để Action hoặc Component xử lý hiển thị
                return Promise.reject(response.data);
            }
        }

        return Promise.reject(error);
    }
);

export default axiosClient;
```

# Cách áp dụng vào Action trong React Router 7
```bash
// pages/Login/LoginAction.js
import axiosClient from "../../api/axios";
import { redirect } from "react-router";

export async function loginAction({ request }) {
    const formData = await request.formData();
    const data = Object.fromEntries(formData);

    try {
        const result = await axiosClient.post("/login", data);
        
        // Vì Interceptor đã return response.data nên result chính là JSON từ Laravel
        localStorage.setItem("laravel_token", result.token);
        
        // Lấy trang cũ để quay lại (nếu có lưu)
        const from = localStorage.getItem("redirect_back") || "/";
        localStorage.removeItem("redirect_back");

        return redirect(from);
    } catch (err) {
        // err ở đây chính là response.data (chứa errors từ Laravel) nhờ Interceptor 422
        return { errors: err.errors };
    }
}
```