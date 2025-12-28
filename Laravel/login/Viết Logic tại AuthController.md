# Viết Logic tại AuthController
```bash
// app/Http/Controllers/AuthController.php

public function login(Request $request) {
    $credentials = $request->validate([
        'email' => 'required|email',
        'password' => 'required',
    ]);

    if (!Auth::attempt($credentials)) {
        return response()->json(['message' => 'Thông tin đăng nhập không chính xác'], 401);
    }

    $user = $request->user();
    
    // KIỂM TRA: Request đến từ Web (Stateful) hay Mobile/Desktop?
    // Cách 1: Kiểm tra xem request có muốn nhận JSON và đến từ domain stateful không
    if ($request->expectsJson() && $this->isStateful($request)) {
        
        // LUỒNG CHO WEB: Tạo Session và trả về Cookie
        $request->session()->regenerate();
        
        return response()->json([
            'user' => $user,
            'type' => 'web_session'
        ]);
        // Laravel Sanctum + statefulApi() sẽ tự động đính kèm Cookie HttpOnly vào response này
    }

    // LUỒNG CHO MOBILE/DESKTOP: Tạo và trả về Plain Text Token
    // Xóa các token cũ nếu muốn (tùy chọn)
    $user->tokens()->delete();
    $token = $user->createToken('mobile_device')->plainTextToken;

    return response()->json([
        'user' => $user,
        'access_token' => $token,
        'type' => 'mobile_token'
    ]);
}

/**
 * Kiểm tra xem request có phải từ Frontend Web được tin tưởng hay không
 */
protected function isStateful(Request $request) {
    return config('sanctum.stateful') && in_array($request->getHost(), config('sanctum.stateful'));
}
```