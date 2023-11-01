# Contents

0. [Home](README.md)
1. [Laravel](#laravel)
2. [Filamentphp](#filamentphp)
   - [Instalation Filamentphp](#installation-filamentphp)
   - [Migrate](#migrate)
   - [Create a user](#create-a-user)
   - [Automatically generating forms and tables](#automatically-generating-forms-and-tables)
   - [Observer](#observer)
3. [Filament spatie roles permissions](#filament-spatie-roles-permissions)
   - [Install Spatie permission package](#install-spatie-permission-package)
   - [Instalation filament spatie roles permissions](#instalation-filament-spatie-roles-permissions)
   - [Generate Permissions](#generate-permissions)
   - [Generating Policies](#generating-policies)
   - [Overriding existing Policies](#overriding-existing-policies)
   - [Adding a Super Admin](#adding-a-super-admin)
   - [Registring role permission](#registring-role-permission)
   - [Navigation group label](#navigation-group-label)

# Laravel

`See more documentation` [here](https://laravel.com/docs/).

Install or create new Laravel project via the Composer command:

```bash
composer create-project laravel/laravel example-app
```

[to top ☝️](#contents)

# Filamentphp

`See more documentation` [here](https://filamentphp.com/docs/).

### Installation Filamentphp

Install the filament and panel builder:

```bash
composer require filament/filament:"^3.0-stable" -W

php artisan filament:install --panels
```

### Migrate

Run the migrate command:

```bash
php artisan migrate
```

### Create a user

Create a new `user account` with the following command, or by using `seedder`:

```bash
php artisan make:filament-user
```

### Automatically generating forms and tables

Before automatically generating forms and tables, must create `model` and `migration`, and run migrate command

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

### Observer

Add observer for deleting file one storage:

```bash
php artisan make:observer UserObserver --model=User
```

register the observer on file `app/Providers/EventServiceProvider.php` on function boot()

```bash
use App\Models\User;
use App\Observers\UserObserver;

public function boot(): void
{
    User::observe(UserObserver::class);
}
```

Add function on file `app\Observer\UserObserver.php` on function update:

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

# Filament spatie roles permissions

[Filament spatie roles permissions plugin](https://filamentphp.com/plugins/tharinda-rodrigo-spatie-roles-permissions) built on top of [Spatie permission package](https://spatie.be/docs/laravel-permission/v5/installation-laravel).

### Install Spatie permission package

`See more documentation` [here](https://spatie.be/docs/laravel-permission/v5/installation-laravel).

Install the package via composer:

```base
composer require spatie/laravel-permission
```

Publish the migration and the config file `config/permission.php`:

```base
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```

Clear config cache:

```base
php artisan optimize:clear
```

Run the migrations:

```base
php artisan migrate
```

### Instalation filament spatie roles permissions

See more documentation [here](https://filamentphp.com/plugins/tharinda-rodrigo-spatie-roles-permissions).

Install the package via composer:

```base
composer require althinect/filament-spatie-roles-permissions
```

Publish the migrations:

```base
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```

Add the plugin to the `AdminPanelProvider`:

```base
use Althinect\FilamentSpatieRolesPermissions\FilamentSpatieRolesPermissionsPlugin;

->plugin(FilamentSpatieRolesPermissionsPlugin::make())
```

Override existing config file:

```base
php artisan vendor:publish --tag="filament-spatie-roles-permissions-config" --force
```

### Generate Permissions

generate Permissions by running:

```base
php artisan permissions:sync
```

### Generating Policies

generated permission and policies

```base
php artisan permissions:sync -P|--policies
```

### Overriding existing Policies

override existing policy classes

```base
php artisan permissions:sync -O|--oep
```

### Adding a Super Admin

Create a Role with the name Super Admin and assign the role to a User
Add the following trait to the User Model

```base
use Althinect\FilamentSpatieRolesPermissions\Concerns\HasSuperAdmin;

class User extends Authenticatable
{
    use HasSuperAdmin;
}
```

In the boot method of the `AuthServiceProvider` add the following

```base
Gate::before(function (User $user, string $ability) {
    return $user->isSuperAdmin() ? true: null;
});
```

### Registring role permission

create model `Role`

```base
use Illuminate\Database\Eloquent\Model;
class Role extends Model

<!-- to -->

use Spatie\Permission\Models\Role as ModelsRole;
class Role extends ModelsRole
```

create model `Permission`

```base
use Illuminate\Database\Eloquent\Model;
class Permission extends Model

<!-- to -->

use Spatie\Permission\Models\Permission as ModelsPermission;
class Permission extends ModelsPermission
```

Register path role and permission on `config/permission.php`

```base
'permission' => App\Models\Permission::class,
'role' => \App\Models\Role::class,
```

### Navigation group label

To change navigation group label on `filament-spatie-roles-permissions.php`

```base
// Default
'navigation_section_group' =>'filament-spatie-roles-permissions::filament-spatie.section.roles_and_permissions',

<!-- to -->

// Modified
'navigation_section_group' => 'Setting',
```

[to top ☝️](#contents)
