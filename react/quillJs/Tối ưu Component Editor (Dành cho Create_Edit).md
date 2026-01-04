# Tối ưu Component Editor (Dành cho Create/Edit)
```jsx
// Editor.jsx (Giữ nguyên bản forwardRef của bạn nhưng đảm bảo import)
import React, { forwardRef, useEffect, useLayoutEffect, useRef } from 'react';
import Quill from 'quill';
import 'quill/dist/quill.snow.css';

const Editor = forwardRef(({ readOnly, defaultValue, onTextChange }, ref) => {
    const containerRef = useRef(null);
    const defaultValueRef = useRef(defaultValue);

    useEffect(() => {
        const container = containerRef.current;
        const editorContainer = container.appendChild(container.ownerDocument.createElement('div'));
        const quill = new Quill(editorContainer, { theme: 'snow' });

        ref.current = quill;
        if (defaultValueRef.current) {
            // Nếu defaultValue là Delta (JSON), dùng setContents. 
            // Nếu là HTML string, dùng clipboard.dangerouslyPasteHTML(defaultValue)
            quill.setContents(defaultValueRef.current); 
        }

        quill.on('text-change', () => onTextChange?.());

        return () => {
            ref.current = null;
            container.innerHTML = '';
        };
    }, [ref]);

    return <div ref={containerRef}></div>;
});

export default Editor;
```
# Cách sử dụng trong Form (Create/Edit)
```jsx
import { useSubmit, Form } from 'react-router';
import Editor from './Editor';

export function CreateOrEditPost({ initialData }) {
    const quillRef = useRef(null);
    const submit = useSubmit();

    const handleSubmit = (event) => {
        event.preventDefault();
        const formData = new FormData(event.currentTarget);
        
        if (quillRef.current) {
            // Lưu cả 2 định dạng
            formData.append('content_delta', JSON.stringify(quillRef.current.getContents()));
            formData.append('content_html', quillRef.current.root.innerHTML);
        }

        submit(formData, { method: "post" });
    };

    return (
        <Form onSubmit={handleSubmit}>
            <input type="text" name="title" defaultValue={initialData?.title} />
            
            {/* Editor chỉ render ở đây */}
            <Editor 
                ref={quillRef} 
                defaultValue={initialData ? JSON.parse(initialData.content_delta) : null} 
            />
            
            <button type="submit">Lưu lại</button>
        </Form>
    );
}
```
# Cách hiển thị trong List và Show (Chỉ dùng HTML)
```jsx
// PostDetail.jsx (Trang Show)
import 'quill/dist/quill.snow.css'; // Quan trọng: Vẫn cần CSS để hiển thị đúng format

export function PostDetail({ post }) {
    return (
        <article className="post-container">
            <h1>{post.title}</h1>
            
            {/* Render trực tiếp HTML từ Laravel, bọc trong lớp 'ql-editor' */}
            <div 
                className="ql-snow" // Bọc ngoài bằng theme
            >
                <div 
                    className="ql-editor" // Quan trọng: Class này kích hoạt style của Quill
                    dangerouslySetInnerHTML={{ __html: post.content_html }} 
                />
            </div>
        </article>
    );
}
```
# Cài đặt thư viện làm sạch dữ liệu (Bắt buộc)
> composer require mews/purifier

## Tạo Trait HasQuillContent
```php
<?php

namespace App\Traits;

use Illuminate\Support\Str;
use Purifier;

trait HasQuillContent
{
    /**
     * Hàm helper để xử lý và lưu dữ liệu từ Quill
     */
    public function setQuillContent(string $delta, string $html)
    {
        $this->attributes['content_delta'] = $delta; // Lưu JSON gốc
        
        // Làm sạch HTML trước khi lưu để chống XSS
        $this->attributes['content_html'] = Purifier::clean($html);
        
        // Tự động tạo đoạn tóm tắt (excerpt) nếu Model có cột này
        if (\Schema::hasColumn($this->getTable(), 'excerpt')) {
            $plainText = strip_tags($html);
            $this->attributes['excerpt'] = Str::limit(html_entity_decode($plainText), 160);
        }
    }
}
```
## Cách 1: Đổ dữ liệu bằng Delta (Khuyên dùng cho Edit)
### Trong component Editor.jsx
```jsx
// Khi khởi tạo Editor
useEffect(() => {
    // ... khởi tạo quill ...
    
    if (defaultValueRef.current) {
        // defaultValue ở đây là Object Delta
        quill.setContents(defaultValueRef.current);
    }
}, []);
```
### Trong trang EditPost.jsx
```jsx
export default function EditPost({ postFromLaravel }) {
    // Giả sử Laravel trả về postFromLaravel.content_delta là một chuỗi JSON
    const initialDelta = JSON.parse(postFromLaravel.content_delta);

    return (
        <Editor 
            ref={quillRef} 
            defaultValue={initialDelta} 
        />
    );
}
```
## 2. Cách 2: Đổ dữ liệu bằng HTML (Dùng khi chỉ có HTML)
### Trong component Editor.jsx
```jsx
useEffect(() => {
    // ... khởi tạo quill ...

    if (defaultValueRef.current) {
        // Nếu defaultValue là chuỗi "<p>Chào</p>"
        quill.clipboard.dangerouslyPasteHTML(defaultValueRef.current);
    }
}, []);
```