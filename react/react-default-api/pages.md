# ErrorBoundary
```bash
import { useRouteError, isRouteErrorResponse } from "react-router"

// pages/ErrorBoundary.jsx
export default function ErrorBoundary() {
    const error = useRouteError();

    // Kiểm tra nếu là lỗi từ Response (ví dụ: 404, 401, 500)
    if (isRouteErrorResponse(error)) {
        return (
            <div>
                <h3>{error.status} || {error.statusText}</h3>
                <p>{error.data}</p>
            </div>
        )
    }
    return (
        <div>
            <h3>Lỗi không xác định</h3>
            <p>{error.message || "Đã có lỗi xảy ra, vui lòng thử lại."}</p>
        </div>
    )
}
```
# Login
```bash
import { useState } from "react";
import { useNavigate, useLocation } from "react-router";

// pages/Login.jsx
export default function Login() {
    // 1. Tạo state để lưu thông báo lỗi
    const [error, setError] = useState('');
    // khoi tao ham navigate
    const navigate = useNavigate();
    const location = useLocation();

    // lấy địa chit trang trươc đó từ state
    const from = location.state?.from ?? '/';
    const handleSubmit = async (e) => {
        e.preventDefault();
        setError(''); // xoa thong bao cu
        const formData = new FormData(e.currentTarget);
        //console.log(formData.get('username'))
        const data = Object.fromEntries(formData.entries());
        //console.log(data)
        try {
            const response = await fetch('/api/login', {
                method: 'post',
                headers: {
                    'Accept': 'application/json'
                },
                body: formData
            });
            const result = await response.json();
            if (result.errors) {
                const arrErrors = Object.values(result.errors);
                //const errMess = arrErros.map(err => `<p>${err}<p/>`).join('')
                // luu truc tiep mang jsx vao state
                // 2. Lưu trực tiếp mảng JSX vào state
                const errElements = arrErrors.map((err, index) => (
                    <p key={index}>{err}</p>
                ));
                setError(errElements)
                //console.log(arrErrors)
                return
            }
            if (response.ok) {
                console.log(result)
                localStorage.setItem('laravel_token', result.token)
                return navigate(from, { replace: true });
                //return navigate('/')
            }
        } catch (err) {
            console.error(err)
        }
    }
    return (
        <form onSubmit={handleSubmit}>
            Username: <input type="text" name="username" /><br />
            Password: <input type="password" name="password" /> <br />
            <button type="submit">Login React</button>
            <div className="err-mess" style={{ color: 'red', marginTop: '10px' }}>
                {error}
            </div>
        </form>
    )
}
```
# Post
```bash
import { Outlet } from "react-router";

export default function Posts() {
    return (
        <div><Outlet /></div>
    )
}
```
# PostIndex
```bash
import { Link, useFetcher, useLoaderData } from "react-router"

export default function PostIndex() {
    const posts = useLoaderData();
    const postsData = posts.data ?? [];
    const fetcher = useFetcher();

    const clickBtnDestroy = (id) => {
        if (confirm("Ban co chac chan xoa")) {
            fetcher.submit(null, {
                method: 'DELETE',
                action: `/posts/${id}/destroy`
            });
        }
    }

    return (
        <div>
            {postsData.map(post => {
                // ĐẶT LOGIC Ở ĐÂY (Phải có dấu ngoặc nhọn { ở trên)
                const isDeletingThisPost =
                    (fetcher.state === 'submitting' || fetcher.state === 'loading') &&
                    fetcher.formAction === `/posts/${post.id}/destroy`;

                // tat ca nt delete đều disable
                //const isDeletingThisPost = fetcher.state === 'submitting' fetcher.state === 'loading';

                // PHẢI CÓ LỆNH RETURN ĐỂ TRẢ VỀ GIAO DIỆN
                return (
                    <div key={post.id} style={{ opacity: isDeletingThisPost ? 0.5 : 1 }}>
                        <h3><Link to={`/posts/${post.id}`}>{post.title}</Link></h3>

                        {/* Lưu ý: Dùng arrow function () => clickBtnDestroy(id) để không bị tự động chạy */}
                        <button
                            onClick={() => clickBtnDestroy(post.id)}
                            disabled={isDeletingThisPost}
                        // style={{
                        //     backgroundColor: isDeletingThisPost ? '#ccc' : '#ff4d4f',
                        //     color: isDeletingThisPost ? '#666' : 'white', // Ép màu chữ rõ ràng
                        //     cursor: isDeletingThisPost ? 'not-allowed' : 'pointer'
                        // }}
                        >
                            {isDeletingThisPost ? 'Deleting...' : 'Delete'}
                        </button>

                        {fetcher.data?.error && isDeletingThisPost && (
                            <p style={{ color: 'red' }}>{fetcher.data.error}</p>
                        )}
                        <p>{post.content}</p>
                        <hr />
                    </div>
                );
            })}
        </div>
    )
}
```
# PostShowId
```bash
import { Link, useLoaderData, useSubmit } from "react-router"

export default function PostShowId() {
    const data = useLoaderData();
    const submit = useSubmit();
    // btn destroy
    const clickBtnDestroy = () => {
        if (confirm("Ban co chac chan xoa")) {
            submit(null, { method: 'DELETE', action: `/posts/${data.id}/destroy` })
        }
    }
    //console.log(data)
    return (
        <div>
            <h3>{data.title}</h3>
            <Link to={`/posts/${data.id}/edit`}>
                <button>Edit</button>
            </Link>
            <span style={{ padding: "0 5px" }}></span>
            <button onClick={clickBtnDestroy}>Destroy</button>
            <div>{data.content}</div>
        </div>
    )
}
```
# PostStore
```bash
import { Form, useActionData, useNavigation } from "react-router";

export default function PostStore() {
    const data = useActionData();
    // Sử dụng Optional Chaining (?.) hoặc kiểm tra data tồn tại
    const errors = data?.errors ?? {}
    //console.log(Object.entries(errors))
    // disable submit
    const navigation = useNavigation();
    // kiem tra trang thai submitting
    const isSubmitting = navigation.state === 'submitting' || navigation.state === 'loading';
    return (
        <div>
            <Form method='post'>
                Title: <input type="text" name="title" /> <br />
                Content: <textarea name="content" rows="2"></textarea> <br />
                <button type="submit" disabled={isSubmitting}>
                    {isSubmitting ? 'Creating.....' : 'Create Post'}
                </button>
            </Form>
            <div>
                {Object.entries(errors).map(([key, err]) =>
                    <p style={{ color: 'red' }} key={key}>{err[0]}</p>
                )}
            </div>
        </div>

    )
}
```
# PostEdit
```bash
import { Form, useActionData, useLoaderData, useNavigate, useNavigation } from "react-router";

export default function PostEdit() {
    const data = useLoaderData();
    const action = useActionData();
    const errors = action?.errors ?? {}
    //console.log(errs)
    // xu ly submit + cancel
    // navigate(-1) -> tra ve trang truoc do
    // navigate("/posts") -> tra ve route
    const navigate = useNavigate();
    const navigation = useNavigation();
    // is submitting -> disable btn
    const isSubmitting = navigation.state === 'submitting';
    return (
        <div>
            <Form method='put'>
                Title: <input type="text" name="title" defaultValue={data.title} /><br />
                Content: <textarea name="content" rows="3" defaultValue={data.content}></textarea><br />
                <button type="submit" disabled={isSubmitting}>
                    {isSubmitting ? "Uploading..." : "Upload"}
                </button><br />
                <button type="button" onClick={() => navigate(-1)}>Cancel</button>
            </Form>
            <div>
                {Object.entries(errors).map(([key, err]) =>
                    <p style={{ color: 'red' }} key={key}>{err[0]}</p>
                )}
            </div>
        </div>
    )
}
```
#
```bash

```