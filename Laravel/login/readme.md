# php artisan install:api
php artisan install:api
# Issuing API Tokens
'Authorization': 'Bearer ' + token

https://laravel.com/docs/12.x/sanctum#issuing-api-tokens

## createToken method
$token = $request->user()->createToken($request->token_name);

return ['token' => $token->plainTextToken];

```bash
if (Auth::attempt($credentials)) {
                $user = Auth::user();
                $token = $user->createToken($user->username)->plainTextToken;
                return response()->json(['token' => $token], 200);
            }
```
# Middleware
->middleware(['auth:sanctum'])
```bash

Route::get('/user', fn(Request $request) => $request->user())
    ->middleware('auth:sanctum');
Route::get('/login', fn(Request $request) => abort(403))->name('login');
Route::post('/login', [AuthApiController::class, 'login']);

// check-auth
Route::middleware('auth:sanctum')->group(function () {
    Route::get('/check-auth', fn(Request $request) => response('',200));
    Route::post('/logout', [AuthApiController::class,'logout']);
});
```
# Revoking Tokens
```bash
// Revoke all tokens...
$user->tokens()->delete();
 
// Revoke the token that was used to authenticate the current request...
$request->user()->currentAccessToken()->delete();
 
// Revoke a specific token...
$user->tokens()->where('id', $tokenId)->delete();
```
# SPA Authentication headers -> RequestForm
Accept: application/json
# Sanctum Middleware -> use session
https://laravel.com/docs/12.x/sanctum#sanctum-middleware
```bash
->withMiddleware(function (Middleware $middleware): void {
    $middleware->statefulApi();
})
```
# CSRF Protection
axios.get('/sanctum/csrf-cookie')

axios.defaults.withCredentials = true;

axios.defaults.withXSRFToken = true;