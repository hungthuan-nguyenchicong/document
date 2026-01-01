# authMiddleware
```bash
// middlewares/authMiddleware.jsx
import { redirect } from "react-router";

// middlewares/authMiddleware.jsx

// Chỉ cần khai báo hàm và export nó
async function authMiddleware({ request }, next) {
    const laravelToken = localStorage.getItem('laravel_token');
    const params = new URL(request.url).pathname;
    if (!laravelToken) {
        //throw redirect('login');
        // "/login", { state: { from: location.pathname }
        //throw redirect(`/login?from=${params}`);
        throw redirect("/login", { state: { from: location.params } })
    }
    const response = await next()
    return null;
}

export { authMiddleware }
```