# Root
```bash
import { Outlet } from "react-router";
import App from "./componets/App";

export default function Root() {
  return (
    <>
    <App />
    <Outlet />
    </>
  )
}
```
# App
```bash
import Sidebar from "./Sidebar";

export default function App() {
  return (
    <Sidebar />
  )
}
```
# Sidebar
```bash
import { Link } from "react-router";

// componets/Sidebar.jsx
export default function Sidebar() {
    return (
        <aside>
            <ul>
                <li>
                    <Link to="/">Home</Link>
                </li>
                <hr />
                <li>
                    <Link to="/posts">Posts</Link>
                    <ul>
                        <li>
                            <Link to="/posts">Post Index</Link>
                        </li>
                        <li>
                            <Link to="/posts/store">Post Store</Link>
                        </li>
                        <li>
                            <Link to="/posts/1">Post show Id test = 1</Link>
                        </li>
                    </ul>
                </li>
                <hr />
            </ul>
        </aside>
    )
}
```
## tham khao logout
```bash
// import { Navigate } from "react-router";

// componets/ButtonLogout.jsx
export default function ButtonLogout() {
    const handleLogout = async () => {
        try {
            const response = await fetch('/api/logout', {
                method: 'post',
                headers: {
                    'Accept': 'application/json',
                    'Authorization': 'Bearer ' + localStorage.getItem('laravel_token')
                }
            })
            if (response.ok) {
                // xoa du lieu dang nhap
                //localStorage.removeItem('laravel_token');
                // neu session
                //sessionStorage.clear();

                // chuyen huong -> SPA react
                //Navigate()
                // 4. Làm mới trang để xóa hoàn toàn trạng thái cũ (tùy chọn)
                // window.location.reload();
                return window.location.href = '/login/';
            }
        } catch (err) {
            console.error(err)
        }
    }
    return (
        <button onClick={handleLogout}>Logout</button>
    )
}
```