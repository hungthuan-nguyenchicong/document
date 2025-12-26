# install
https://vite.dev/guide/#manual-installation

# node
```bash
npm init
```
# vite
```bash
npm install -D vite
```
## package.json
```bash
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

## fix
"dev-vite": "cd vite && vite",
"dev": "npm run dev-vite",