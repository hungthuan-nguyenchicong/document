# default
npm i react-router
# main.jsx
```bash
// main.jsx
import { createRoot } from 'react-dom/client';
import { RouterProvider } from 'react-router/dom';
import { routes } from './routes';

createRoot(document.getElementById('root')).render(
    <RouterProvider router={routes} />
);
```
# routes.jsx
```bash
// routes.jsx
import { createBrowserRouter } from "react-router";

const routes = createBrowserRouter([
    {
        path: '/',
        element: <div>Hello word</div>
    }
]);

export { routes }
```