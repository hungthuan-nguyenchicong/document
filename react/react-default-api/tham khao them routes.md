# v1
## Tạo file route riêng cho từng Module
```bash
// src/routes/postRoutes.jsx
import Posts from "../pages/Posts";
import PostIndex from "../pages/posts/PostIndex";
import PostStore from "../pages/posts/PostStore";
import { postIndexLoader } from "../loaders/posts/postIndexLoader";
import { postStoreAction } from "../actions/posts/postStoreAction";
// ... import các loader/action khác

export const postRoutes = {
    path: 'posts',
    Component: Posts,
    children: [
        { index: true, loader: postIndexLoader, Component: PostIndex },
        { path: 'store', action: postStoreAction, Component: PostStore },
        { path: ':id', /* ... */ },
        { path: ':id/edit', /* ... */ },
        { path: ':id/destroy', /* ... */ }
    ]
};
```
## Tổng hợp các Route tại file routes.jsx chính
```bash
// src/routes.jsx
import { createBrowserRouter } from "react-router";
import Root from "./Root";
import Login from "./Login";
import { postRoutes } from "./routes/postRoutes"; // Import module Posts
// import { userRoutes } from "./routes/userRoutes"; // Ví dụ Module Users

const routes = createBrowserRouter([
    {
        path: '/',
        middleware: [authMiddleware],
        Component: Root,
        errorElement: <ErrorBoundary />,
        children: [
            { index: true, Component: Home },
            
            // Sử dụng toán tử spread hoặc đưa thẳng object vào
            postRoutes, 
            
            // userRoutes, // Thêm các module khác tại đây
        ],
    },
    {
        path: "login",
        Component: Login,
        errorElement: <ErrorBoundary />
    },
]);

export { routes };
```
## Mẹo nâng cao: Nhóm nhiều Module vào một mảng
```bash
// gom lại thành một danh sách các tài nguyên
const resourcesRoutes = [
    postRoutes,
    userRoutes,
    productRoutes,
    categoryRoutes
];

// Trong children của Root
children: [
    { index: true, Component: Home },
    ...resourcesRoutes // Dùng spread operator để "rải" các object vào mảng con
]
```
# v2 doc them
## Trường hợp 1: Nếu postRoutes là một Đối tượng (Object)
```bash
// postRoutes.jsx
export const postRoutes = {
    path: 'posts',
    children: [...]
};

// routes.jsx
children: [
    { index: true, Component: Home },
    postRoutes, // Chèn trực tiếp đối tượng vào mảng
]
```
## Trường hợp 2: Nếu postRoutes là một Mảng (Array) - Cần dùng Spread ...
```bash
// postRoutes.jsx
export const postRoutes = [
    { path: 'posts', Component: PostIndex },
    { path: 'posts/store', Component: PostStore },
];

// routes.jsx
children: [
    { index: true, Component: Home },
    ...postRoutes, // "Rải" từng phần tử của mảng postRoutes vào đây
]
```
## Trường hợp 3: Nhóm nhiều Module (Gọn nhất cho dự án lớn)
```bash
// gom tất cả module lại thành 1 mảng
const allModules = [postRoutes, userRoutes, categoryRoutes];

const routes = createBrowserRouter([
    {
        path: '/',
        Component: Root,
        children: [
            { index: true, Component: Home },
            ...allModules // Rải tất cả các Object module vào mảng children
        ]
    }
]);
```