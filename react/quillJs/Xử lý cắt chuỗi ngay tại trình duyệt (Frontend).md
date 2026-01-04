# Xử lý cắt chuỗi ngay tại trình duyệt (Frontend)
```jsx
const handleSubmit = (event) => {
    event.preventDefault();
    const formData = new FormData(event.currentTarget);
    
    if (quillRef.current) {
        const delta = quillRef.current.getContents();
        const html = quillRef.current.root.innerHTML;

        // --- BƯỚC CẮT CHUỖI TẠI FRONTEND ---
        // 1. Lấy text thuần (không thẻ HTML)
        const plainText = quillRef.current.getText().trim();
        // quill.getText(0, 10)
        // 2. Cắt ngắn (ví dụ 160 ký tự)
        const excerpt = plainText.length > 160 
            ? plainText.substring(0, 160) + '...' 
            : plainText;

        // 3. Gán vào FormData (Backend sẽ nhận cột 'excerpt')
        formData.append('content_delta', JSON.stringify(delta));
        formData.append('content_html', html);
        formData.append('excerpt', excerpt); 
    }

    submit(formData, { method: "post" });
};
```
## Hàm cắt chuỗi thông minh (Smart Truncate)
```jsx
const getSmartExcerpt = (quill, maxLength = 160) => {
    // 1. Lấy dư ra một chút (ví dụ 170 ký tự) để tìm điểm ngắt từ
    const text = quill.getText(0, maxLength + 10).trim();

    if (text.length <= maxLength) return text;

    // 2. Tìm vị trí khoảng trắng cuối cùng trong phạm vi maxLength
    // Việc này giúp chúng ta ngắt đúng ở khoảng trắng thay vì giữa từ
    const lastSpace = text.lastIndexOf(' ', maxLength);

    // 3. Nếu tìm thấy khoảng trắng, cắt đến đó. Nếu không (từ quá dài), cắt đúng maxLength
    return text.substring(0, lastSpace > 0 ? lastSpace : maxLength) + "...";
};
```