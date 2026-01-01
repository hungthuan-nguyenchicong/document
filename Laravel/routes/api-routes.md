# php artisan
## install:api
php artisan install:api
## code
```bash
<?php

use App\Http\Controllers\PostController;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\AuthApiController;

Route::get('/user', fn(Request $request) => $request->user())
    ->middleware('auth:sanctum');
Route::get('/login', fn(Request $request) => response()->noContent(403))->name('login');
Route::post('/login', [AuthApiController::class, 'login']);

// check-auth
Route::middleware('auth:sanctum')->group(function () {
    Route::get('/check-auth', fn(Request $request) => response('', 200));
    Route::post('/logout', [AuthApiController::class, 'logout']);

    //https://laravel.com/docs/12.x/controllers#api-resource-routes
    Route::apiResource('posts', PostController::class);
});

```