# laravel-toast
Simple toast messages for Laravel 5.


## Installation
1. Run `composer require grimthorr/laravel-toast` to include this in your project.
2. Add `'Grimthorr\LaravelToast\ServiceProvider'` to `providers` in `config/app.php`.

  ```php
  'providers' => array(
    // ...
    'Grimthorr\LaravelToast\ServiceProvider',
  ),
  ```

3. Add `'Toast' => 'Grimthorr\LaravelToast\Facade'` to `aliases` in `config/app.php`.

  ```php
  'aliases' => array(
    // ...
    'Toast' => 'Grimthorr\LaravelToast\Facade',
  ),
  ```

4. *Optional*: Run `php artisan vendor:publish --provider="Grimthorr\LaravelToast\ServiceProvider" --tag="config"` to publish the config file.
5. *Optional*: Modify the published configuration file located at `config/laravel-toast.php` to your liking.
6. *Optional*: Run `php artisan vendor:publish --provider="Grimthorr\LaravelToast\ServiceProvider" --tag="views"` to publish the views.
7. *Optional*: Modify the published views located at `resources/views/vendor/toast` to your liking.


## Configuration
Pop open `config/laravel-toast.php` to adjust package configuration. If this file doesn't exist, run `php artisan vendor:publish --provider="Grimthorr\LaravelToast\ServiceProvider" --tag="config"` to create the default configuration file.

```php
return array(
  'levels' => array(
    'info' => 'info',
    'success' => 'success',
    'error' => 'error',
    'warning' => 'warning',
    'default' => 'info'
  ),
);
```

#### Levels
Specify the class sent to the view for each level. For example calling the `info` method would send the `info` class to the view. If you use [Bootstrap](http://getbootstrap.com/), you could set this to `alert alert-info` for ease of use in the view.

You can create a custom method here by passing a new level name and class. For example: `'help' => 'help'` will allow you to call `Toast::help($message)`. Alternatively, you can use the `Toast::message($message, $level)` method instead.

#### Views
This package includes a couple of views to get you started, they can be published to your resources directory using `php artisan vendor:publish --provider="Grimthorr\LaravelToast\ServiceProvider" --tag="views"` or called straight from the package by including them in a Blade template: `@include('toast::messages')`.

```html
@if(Session::has('toasts'))
  @foreach(Session::get('toasts') as $toast)
    <div class="alert alert-{{ $toast['level'] }}">
      <button type="button" class="close" data-dismiss="alert" aria-hidden="true">&times;</button>
      
      {{ $toast['message'] }}
    </div>
  @endforeach
@endif
```


## Usage
Use the Toast facade (`Toast::`) or the helper function (`toast()->`) to access the methods in this package. You can also chain multiple messages together using method chaining: `toast()->success('done')->info('hello')`.

#### Message
```php
Toast::message('message', 'level');
toast()->message('message', 'level');
toast('message');
```
Add a toast to the session. Using `toast('message')` will use the default level.

#### Info
```php
Toast::info('message');
toast()->info('message');
```
Add a toast with the `info` level.

#### Success
```php
Toast::success('message');
toast()->success('message');
```
Add a toast with the `success` level.

#### Error
```php
Toast::error('message');
toast()->error('message');
```
Add a toast with the `error` level.

#### Warning
```php
Toast::warning('message');
toast()->warning('message');
```
Add a toast with the `warning` level.


## Example
These examples are using the default configuration.

#### Using the facade to send an error message
The following adds an error toast to the session and then redirects to `home`.
```php
// Create the message
Toast::error('oops');

// Return a HTTP response to initiate the new session
return Redirect::to('home');
```

#### Using method chaining to create multiple toasts
The following adds an error and info toast to the session and then redirects to `home`.
```php
// Create the message
Toast::error('oops')
  ->info('hello');

// Return a HTTP response to initiate the new session
return Redirect::to('home');
```

#### Using the helper function to send a message with a custom level
The following adds a help toast to the session and then redirects to `home`.
```php
// Create the message
Toast::message('example', 'help');

// Return a HTTP response to initiate the new session
return Redirect::to('home');
```


## Finally

#### Contributing
Feel free to create a fork and submit a pull request if you would like to contribute.

#### Bug reports
Raise an issue on GitHub if you notice something broken.

#### Credits
Based loosely on https://github.com/laracasts/flash.
