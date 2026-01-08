# npm add -D sass-embedded
> npm add -D sass-embedded
## vite.config.js
```vite.config.js
// vite.config.js
export default defineConfig({
  css: {
    preprocessorOptions: {
      scss: {
        api: 'modern-compiler', // Khuyên dùng cho sass-embedded
      },
    },
  },
});
```