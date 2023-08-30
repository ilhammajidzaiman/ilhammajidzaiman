# Contents

1. [Laravel](#laravel)
    - [Instalation Laravel](#installation-laravel)
2. [Filamentphp](#filamentphp)
    - [Instalation Filamentphp](#installation-filamentphp)
    - [Migrate](#migrate)
    - [Create a user](#create-a-user)
    - [Automatically generating forms and tables](#automatically-generating-forms-and-tables)
    - [Observer](#observer)

# Laravel

See more documentation of `laravel` [here](https://laravel.com/docs/).

## Installation Laravel

Install larvel using composer:

```bash
composer create-project laravel/laravel example-app
```

[to top ☝️](#contents)

# Filamentphp

See more documentation of `filamentphp` [here](https://filamentphp.com/docs/).

## Installation Filamentphp

Since livewire v3 is still in beta, set the minimum-stability in your composer json to dev:

```bash
"minimum-stability": "dev",
```

Install the filament panel builder:

```bash
composer require filament/filament:"^3.0-stable" -W

php artisan filament:install --panels
```

## Migrate

Run the migrate command:

```bash
php artisan migrate
```

## Create a user

Create a new `user account` with the following command, or by using `seedder`:

```bash
php artisan make:filament-user
```

## Automatically generating forms and tables

Create resource:

```bash
php artisan make:filament-resource User --generate
```

Create resource with `softdelete`:

```bash
php artisan make:filament-resource User --generate --soft-deletes
```

Add `view` page for view detail data without modal:

```bash
php artisan make:filament-page ViewUser --resource=UserResource --type=ViewRecord
```

## Observer

Add observer for deleting file one storage:

```bash
php artisan make:observer UserObserver --model=User
```

Add function on file `app\Observer\UserObserver.php` on update:

```bash
public function updated(User $user): void
    {
        if ($user->isDirty('file')) {
            Storage::disk('public')->delete($user->getOriginal('file'));
        }
    }
```

On delete or forceDelete if using `softdelete`:

```bash
public function forceDeleted(User $user): void
    {
        if (!is_null($user->file)) {
            Storage::disk('public')->delete($user->file);
        }
    }
```

[to top ☝️](#contents)
