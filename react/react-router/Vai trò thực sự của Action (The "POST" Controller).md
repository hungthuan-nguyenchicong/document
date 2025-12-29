# Bước 1: Định nghĩa Action (Giống Controller xử lý POST)
```bash
// pages/Login.jsx
import { redirect } from "react-router";

export async function loginAction({ request }) {
    const formData = await request.formData(); // Tự động bắt data từ form
    const data = Object.fromEntries(formData);

    const response = await fetch('/api/login', {
        method: 'POST',
        body: JSON.stringify(data),
    });

    const result = await response.json();

    if (result.errors) {
        return { errors: result.errors }; // Trả về lỗi cho UI hứng
    }

    localStorage.setItem('laravel_token', result.token);
    return redirect("/dashboard"); // Xử lý xong thì đuổi đi trang khác
}
```
## Bước 2: Component chỉ lo hiển thị (View)
```bash
import { Form, useActionData, useNavigation } from "react-router";

export default function Login() {
    const actionData = useActionData(); // Hứng lỗi từ "Controller" action trả về
    const navigation = useNavigation(); // Biết được đang "Submitting..." hay không
    
    const isSubmitting = navigation.state === "submitting";

    return (
        <Form method="post"> {/* <--- Dùng Form của Router, không cần onSubmit */}
            <input name="username" />
            <input name="password" type="password" />
            
            <button disabled={isSubmitting}>
                {isSubmitting ? "Logging in..." : "Login"}
            </button>

            {actionData?.errors && <p>{actionData.errors.message}</p>}
        </Form>
    );
}
```

## Viết lại sơ đồ trong routes.jsx
// routes.jsx
const routes = [
  {
    path: "/dashboard",
    Component: DashboardLayout,
    // 1. Tầng bảo vệ (Chạy đầu tiên)
    middleware: [authMiddleware], 
    
    // 2. Tầng chuẩn bị dữ liệu (Chạy sau middleware, trước khi hiện UI)
    loader: async ({ request }) => {
       return await fetch("/api/user-stats");
    },

    // 3. Tầng xử lý hành động (Chỉ chạy khi có <Form method="post">)
    action: async ({ request }) => {
       const data = await request.formData();
       return await updateProfile(data);
    },

    children: [
      {
        index: true,
        Component: DashboardHome, // UI cuối cùng hiện ra ở đây
      }
    ]
  }
];
