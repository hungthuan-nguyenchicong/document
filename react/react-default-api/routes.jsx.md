# routes.jsx
```bash
// routes.jsx
// css
import './css/main.css';
// react
import { createBrowserRouter } from "react-router";
import { authMiddleware } from "./middlewares/authMiddleware";
import Root from "./Root";
import Home from './pages/Home'
import Login from "./Login";
import ErrorBoundary from "./pages/ErrorBoundary";
import Posts from "./pages/Posts";
import PostIndex from "./pages/posts/PostIndex";
import PostStore from "./pages/posts/PostStore";
import { postIndexLoader } from "./loaders/posts/postIndexLoader";
import PostShowId from "./pages/posts/PostShowId";
import { postShowIdLoader } from "./loaders/posts/postShowIdLoader";
import { postStoreAction } from "./actions/posts/postStoreAction";
import PostEdit from "./pages/posts/PostEdit";
import { postEditAction } from "./actions/posts/postEditAction";
import { postDestroyAction } from "./actions/posts/postDestroyAcation";

const routes = createBrowserRouter([
    {
        path: '/',
        middleware: [authMiddleware],
        Component: Root,
        errorElement: <ErrorBoundary />,
        children: [
            {
                index: true,
                Component: Home,
                errorElement: <ErrorBoundary />
            },
            {
                path: 'posts',
                Component: Posts,
                errorElement: <ErrorBoundary />,
                children: [
                    {
                        index: true,
                        loader: postIndexLoader,
                        Component: PostIndex,
                        errorElement: <ErrorBoundary />

                    },
                    {
                        path: ':id',
                        loader: postShowIdLoader,
                        Component: PostShowId,
                        errorElement: <ErrorBoundary />
                    },
                    {
                        path: 'store',
                        action: postStoreAction,
                        Component: PostStore,
                        errorElement: <ErrorBoundary />
                    },
                    {
                        path: ':id/edit',
                        action: postEditAction,
                        loader: postShowIdLoader,
                        Component: PostEdit,
                        errorElement: <ErrorBoundary />
                    },
                    {
                        path: ':id/destroy',
                        action: postDestroyAction,
                        errorElement: <ErrorBoundary />
                    }

                ]
            },

        ],

    },
    {
        path: "login",
        Component: Login,
        errorElement: <ErrorBoundary />
    },

]);

export { routes }
```