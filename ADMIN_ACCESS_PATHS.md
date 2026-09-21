# Admin Access Flow: Relative Paths

This document lists the relative paths involved in the authenticated user/admin dashboard flow.

## Request flow

1. A signed-in user opens `/home`.
2. The route in `routes/web.php` calls `AdminController@index`.
3. `app/Http/Controllers/AdminController.php` reads the authenticated user's `usertype`.
4. The controller returns one of these Blade views:
   - `user` -> `resources/views/dashboard.blade.php`
   - `admin` -> `resources/views/admin/index.blade.php`
5. If the user is not authenticated or has an unsupported user type, access does not continue to an admin dashboard.

## Relative paths

| Purpose | Relative path |
| --- | --- |
| Web route | `routes/web.php` |
| Controller | `app/Http/Controllers/AdminController.php` |
| User model | `app/Models/User.php` |
| Users table migration | `database/migrations/0001_01_01_000000_create_users_table.php` |
| Regular user dashboard | `resources/views/dashboard.blade.php` |
| Admin dashboard | `resources/views/admin/index.blade.php` |
| Shared application layout | `resources/views/components/app-layout.blade.php` |
| Authentication configuration | `config/auth.php` |

## Route and controller mapping

The route is defined in `routes/web.php`:

```php
Route::get('/home', [AdminController::class, 'index'])->name('home');
```

The controller class is imported with:

```php
use App\Http\Controllers\AdminController;
```

The controller method uses the Laravel view names below:

| View name in PHP | Blade file path |
| --- | --- |
| `view('dashboard')` | `resources/views/dashboard.blade.php` |
| `view('admin.index')` | `resources/views/admin/index.blade.php` |

Laravel converts dots in a view name to directory separators. Therefore, `admin.index` points to the `admin/index.blade.php` file inside `resources/views`.

## User type data

The `usertype` value is defined in these files:

- `database/migrations/0001_01_01_000000_create_users_table.php` creates the database column and defaults it to `user`.
- `app/Models/User.php` includes `usertype` in the `$fillable` array.
- `app/Http/Controllers/AdminController.php` checks for `user` and `admin`.

Expected values:

| `usertype` value | Result |
| --- | --- |
| `user` | Opens the regular dashboard |
| `admin` | Opens the admin dashboard |
| Any other value | Redirects back |

## Important syntax

Use the authenticated user method with parentheses:

```php
$usertype = Auth::user()->usertype;
```

`Auth::user()` returns the current user model. The `usertype` property is then read from that model.
