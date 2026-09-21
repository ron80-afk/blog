# Laravel Project Setup on Another Windows PC

This project is a Laravel 12 application with a Vite frontend. These steps set it up on another Windows PC using XAMPP.

## 1. Install the required software

Install the following:

- Git
- XAMPP with PHP 8.2 or newer
- Composer
- Node.js LTS, which includes npm

Verify the installations in PowerShell:

```powershell
php -v
composer --version
node --version
npm --version
git --version
```

## 2. Clone the repository

Open PowerShell and run:

```powershell
cd C:\xampp\htdocs
git clone YOUR_GITHUB_REPOSITORY_URL blogproject
cd blogproject
```

Replace `YOUR_GITHUB_REPOSITORY_URL` with the repository URL.

## 3. Install PHP dependencies

```powershell
composer install
```

This creates the `vendor` directory from `composer.json` and `composer.lock`.

## 4. Create the environment file

```powershell
copy .env.example .env
php artisan key:generate
```

The `.env` file contains machine-specific settings and should not be committed to Git.

## 5. Configure MySQL in XAMPP

1. Open the XAMPP Control Panel.
2. Start **Apache** and **MySQL**.
3. Open `http://localhost/phpmyadmin`.
4. Create a database named `blogproject`.
5. Open the project `.env` file and set the database values:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=blogproject
DB_USERNAME=root
DB_PASSWORD=
```

The default XAMPP MySQL password is commonly empty. Use the actual password configured on the PC if it is different.

## 6. Create the database tables

Run the Laravel migrations:

```powershell
php artisan migrate
```

The database contents are not copied by Git. New users must be created in the new database.

## 7. Install frontend dependencies

```powershell
npm install
npm run build
```

This creates the frontend assets used by the application.

Create the public storage link if the application uses uploaded files:

```powershell
php artisan storage:link
```

## 8. Run the application

Start Laravel in the project directory:

```powershell
php artisan serve
```

Open the URL shown in the terminal, usually:

```text
http://127.0.0.1:8000
```

For frontend development with live updates, open a second PowerShell window in the project directory and run:

```powershell
npm run dev
```

## Quick setup command

After configuring `.env` and creating the MySQL database, this project also provides:

```powershell
composer run setup
```

That command installs Composer packages, creates the environment file when needed, generates the application key, runs migrations, installs npm packages, and builds frontend assets.

## Files that are not copied from Git

These are generated locally and should not be manually copied between computers:

- `.env`
- `vendor/`
- `node_modules/`
- Database records

Use `composer install`, `npm install`, and `php artisan migrate` to recreate the project environment.

## Troubleshooting

### `php` is not recognized

Add the XAMPP PHP directory to the Windows PATH, commonly:

```text
C:\xampp\php
```

Restart PowerShell after changing PATH.

### Database connection error

Check that MySQL is running in XAMPP and that the database name, username, password, and port in `.env` are correct.

### Missing application key

Run:

```powershell
php artisan key:generate
```

### Frontend styles or scripts are missing

Run:

```powershell
npm install
npm run build
```
