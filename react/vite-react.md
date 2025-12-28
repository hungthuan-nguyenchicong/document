# @vitejs/plugin-react
https://www.npmjs.com/package/@vitejs/plugin-react

npm i @vitejs/plugin-react
# vite.config.js
```bash
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
})
```
# npm install react react-dom
https://www.npmjs.com/package/react-dom

npm install react react-dom
# In the browser
```bash
import { createRoot } from 'react-dom/client';

function App() {
  return <div>Hello World</div>;
}

const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

# react-router
https://www.npmjs.com/package/react-router

npm i react-router

https://reactrouter.com/start/data/installation

# main-rect-router
```bash
// src/main-rect-router.jsx
import { createRoot } from 'react-dom/client';
import { createBrowserRouter } from 'react-router';
import { RouterProvider } from 'react-router/dom';

const router = createBrowserRouter([
    {
        path: '/',
        element: <div>Hello Word</div>
    }
])

function Hello() {
    return (
        <h1>Hello World abc!</h1>
    );
}

createRoot(document.getElementById('root')).render(
    <RouterProvider router={router} />
);
```