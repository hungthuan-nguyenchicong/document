# Controller Auth note
## validated -> class AuthApiLoginRequest extends FormRequest
### note 403
```bash
public function authorize(): bool
    {
        return true;
    }
```
### validate
```bash
public function rules(): array
    {
        return [
            'username' => 'required',
            'password' => 'required'
        ];
    }
```
### validated
$credentials = $request->validated()
## Auth::attempt() -> xac thuc
Auth::attempt($credentials)

$user = Auth::user();
### App\Models\User
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
#### HasApiTokens
use Laravel\Sanctum\HasApiTokens;

use HasFactory, Notifiable, HasApiTokens;
#### Fillble -> add username
```bash
protected $fillable = [
        'name',
        'username',
        'email',
        'password',
    ];
```
#### Hidden Password
```bash
protected $hidden = [
        'password',
        'remember_token',
    ];
```
#### seader -> user admin
```bash
$this->call([
            PostSeeder::class,
        ]);
        User::factory(5)->create();

        User::factory()->create([
            'name' => 'Admin User',
            'username' => 'admin',
            'password' => bcrypt('YOUR_PASSWORD'),
            'email' => 'admin@example.com',
        ]);
```
## createToken
$user = Auth::user();

// note: $token = $user->createToken('any_string')->plainTextToken

$token = $user->createToken($user->username)->plainTextToken

return response()->json(['token'=>$token], 200)

### bat errors theo validate
return response()->json(['errors' => ['TK khong dung']], 401);

## Logout
```bash
public function logout(Request $request): JsonResponse {
    if ($user = $request->user()) {
        $user->currentAccessToken()->delete();
    }
    // (Sử dụng 204 No Content)
    return response()->json(null,204)
}
```
## AuthApiController
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
            return response()->json('', 500);
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
            return response()->json('', 200);
        } catch (\Throwable $e) {
            return response()->json('', 500);
        }
    }
}

```