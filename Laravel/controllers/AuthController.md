# Login -> log out
```bash
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Http\JsonResponse;
use Illuminate\Support\Facades\Log;
use Illuminate\Support\Facades\Auth;
use App\Http\Requests\AuthApiLoginRequest;

class AuthApiController extends Controller
{
    public function login(AuthApiLoginRequest $request): JsonResponse
    {
        try {
            $credentials = $request->validated();
            if (Auth::attempt($credentials)) {
                $user = Auth::user();
                $token = $user->createToken($user->username)->plainTextToken;
                return response()->json(['token' => $token], 200);
            }
            return response()->json([
                'errors' => ['Tài khoản hoặc mật khẩu không đúng']
            ], 401);
            //Log::debug("message");
        } catch (\Throwable $e) {
            //Log::debug("message err");
            //throw $e;
            return response()->json(null, 500);
        }
    }

    public function logout(Request $request): JsonResponse
    {
        try {
            // Thu hồi token hiện tại đang dùng để request
            // $request->user()->currentAccessToken()->delete();
            if ($user = $request->user()) {
                $user->currentAccessToken()->delete();
            }
            // 204 nocontent
            return response()->json(null, 204);
        } catch (\Throwable $e) {
            return response()->json(null, 500);
        }
    }
}

```