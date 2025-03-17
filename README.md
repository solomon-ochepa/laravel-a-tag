# Laravel `<a>` Tag Component

A simple Laravel Blade component for generating anchor (`<a>`) tags with route handling, authorization checks, and active state management.

## Installation

### Create the `a` Component
Run the following command to generate the Blade component:

```sh
php artisan make:component a --view
```

### Update the Component View
Replace the generated resources/views/components/a.blade.php file with the following code:
```blade
@props(['route' => null, 'href' => null, 'can' => null])
@php
    $href = $href ?? ($route && Route::has($route) ? route($route) : null);
    $active = $route && request()->routeIs($route) ? 'active' : '';
@endphp
@if ($href && ($can || Auth::user()->can($can)))
    <a {{ $attributes->merge(['class' => $active]) }} href="{{ $href }}">
        {{ $slot }}
    </a>
@endif
```

### Usage
Use the component in your Blade views:
```blade
<x-a class="item" route="admin">
    <div class="col">
        <ion-icon name="cog"></ion-icon>
        <span>Settings</span>
    </div>
</x-a>
```

### Available Props:
- route (optional) – Named route to generate the link.
- href (optional) – Direct URL (overrides route).
- can (optional) – Authorization ability check.

### Notes
- If both route and href are provided, href takes priority.
- The active class is added if the current route matches.
- If can is set, the link will only be rendered if the user has the necessary permission.
