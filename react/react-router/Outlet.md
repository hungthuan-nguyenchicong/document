# Outlet
## routes.jsx
```bash
// routes.jsx
import { createBrowserRouter } from "react-router";
import Root from "./Root";
import Home from "./pages/Home";
import About from "./pages/About";

const routes = createBrowserRouter([
    {
        path: '/',
        Component: Root,
        children: [
            {
                index: true,
                Component: Home
            },
            {
                path: 'about',
                Component: About
            }
        ]
    }
]);

export { routes }
```
## Root.jsx
```bash
// Root.jsx
import { Outlet } from "react-router"
export default function Root() {
  return (
    <div>
      <h1>Root</h1>
      <Outlet />
    </div>
  )
}
```