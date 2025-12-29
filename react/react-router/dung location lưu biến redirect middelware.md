# Cách 1: Sử dụng URL Search Params (Khuyên dùng)
```bash
import { redirect } from "react-router";

async function authMiddleware({ request }, next) {
    const laravelToken = localStorage.getItem('laravel_token');
    const url = new URL(request.url);
    const pathname = url.pathname;

    if (!laravelToken && pathname !== "/login") {
        // Truyền qua URL để trang Login dễ dàng bắt được bằng useSearchParams
        throw redirect(`/login?from=${pathname}`);
    }
    
    return await next();
}
```

# Cách 2: Sử dụng localStorage (Nếu muốn URL sạch)

```bash
async function authMiddleware({ request }, next) {
    const laravelToken = localStorage.getItem('laravel_token');
    const url = new URL(request.url);

    if (!laravelToken && url.pathname !== "/login") {
        // Lưu tạm vào localStorage
        localStorage.setItem('redirect_from', url.pathname);
        throw redirect("/login");
    }
    
    return await next();
}
```
## Cách sửa để bắt đúng tham số ?from=/home
```bash
import { useState } from "react";
import { useNavigate, useLocation, useSearchParams } from "react-router"; // Import thêm useSearchParams

export default function Login() {
    const [error, setError] = useState('');
    const navigate = useNavigate();
    const [searchParams] = useSearchParams(); // Hook để đọc query string (?from=...)

    // Lấy giá trị từ ?from=/home trên URL
    // Nếu không có, nó sẽ mặc định quay về '/'
    const from = searchParams.get('from') || '/';

    const handleSubmit = async (e) => {
        e.preventDefault();
        setError('');
        const formData = new FormData(e.currentTarget);
        
        try {
            const response = await fetch('/api/login', {
                method: 'post',
                headers: { 'Accept': 'application/json' },
                body: formData
            });
            
            const result = await response.json();
            
            if (result.errors) {
                const arrErrors = Object.values(result.errors);
                setError(arrErrors.map((err, index) => <p key={index}>{err}</p>));
                return;
            }

            if (response.ok) {
                localStorage.setItem('laravel_token', result.token);
                
                // Chuyển hướng về trang 'from' đã bắt được
                return navigate(from, { replace: true });
            }
        } catch (err) {
            console.error("Login error:", err);
            setError(<p>Máy chủ không phản hồi, vui lòng thử lại sau.</p>);
        }
    };

    return (
        <form onSubmit={handleSubmit}>
            Username: <input type="text" name="username" /><br />
            Password: <input type="password" name="password" /> <br />
            <button type="submit">Login React</button>
            <div className="err-mess" style={{ color: 'red', marginTop: '10px' }}>
                {error}
            </div>
        </form>
    );
}
```
## Cập nhật Login.jsx để lấy dữ liệu từ localStorage
```bash
import { useState } from "react";
import { useNavigate } from "react-router";

export default function Login() {
    const [error, setError] = useState('');
    const navigate = useNavigate();

    const handleSubmit = async (e) => {
        e.preventDefault();
        const formData = new FormData(e.currentTarget);
        
        try {
            const response = await fetch('/api/login', {
                method: 'post',
                body: formData
            });
            const result = await response.json();

            if (response.ok) {
                localStorage.setItem('laravel_token', result.token);

                // 1. Lấy đường dẫn đã lưu từ localStorage, mặc định là '/' nếu không có
                const redirectTo = localStorage.getItem('redirect_back') || '/';
                
                // 2. Xóa nó đi sau khi đã lấy để tránh redirect nhầm lần sau
                localStorage.removeItem('redirect_back');

                // 3. Chuyển hướng
                return navigate(redirectTo, { replace: true });
            }
        } catch (err) {
            console.error(err);
        }
    };

    return (
        <form onSubmit={handleSubmit}>
            {/* UI Form của bạn */}
            <button type="submit">Login</button>
        </form>
    );
}
```