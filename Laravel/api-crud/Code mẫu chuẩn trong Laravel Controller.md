# Code mẫu chuẩn trong Laravel Controller
```bash
// TẠO MỚI
public function store(StorePostRequest $request) {
    $post = Post::create($request->validated());
    return response()->json($post, 201); // 201 + Dữ liệu
}

// CẬP NHẬT
public function update(UpdatePostRequest $request, Post $post) {
    $post->update($request->validated());
    return response()->json($post, 200); // 200 + Dữ liệu mới
}

// XÓA
public function destroy(Post $post) {
    $post->delete();
    return response()->json(null, 204); // 204 + Rỗng
}
```