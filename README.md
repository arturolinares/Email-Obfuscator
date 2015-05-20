Email Obfuscator
=====================

A text filter for automatic email obfuscation using the well-established Javascript and a CSS fallback:

- For Javascript-enabled browsers: ROT13 ciphering
- For non-Javascript browsers: CSS reversed text direction

## Contents

- Installation
	- [Common installation step](#common-installation-step)
	- Platform specific steps & usage
		- [Standalone](#standalone)
		- [Laravel 5](#laravel-5)
		- [Twig](#twig)
- [Call for integrations](#call-for-integrations)
- [Credits](#credits)

### Common installation step

Add the library to `composer.json`:

```json
"require": {
    "propaganistas/email-obfuscator": "~.1.0",
}

```

And finally include the supplied Javascript file (`assets/EmailObfuscator.js`) somewhere in your template. Publish the file yourself or use the following CDN link (no uptime guaranteed):

    https://cdn.rawgit.com/Propaganistas/Email-Obfuscator/master/assets/EmailObfuscator.js

### Platform specific steps & usage

- [Standalone](#standalone)
- [Laravel](#laravel)
- [Twig](#twig)

#### Standalone

Include the `src/Obfuscator.php` file somewhere in your project:

    require_once 'PATH_TO_LIBRARY/src/Obfuscator.php';

Parse and obfuscate a string by using the `obfuscateEmail($string)` function.

#### Laravel 5

You have 3 options depending on your use case:

- (recommended) If you want to obfuscate all email addresses that Laravel ever outputs, add the Middleware class to the `$middleware` array in `App\Http\Middleware\Kernel.php`:

        protected $middleware = [
    		'Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode',
    		...
    		'Propaganistas\EmailObfuscator\Laravel\Middleware',
    	];

- If you only want to have specific controller methods return obfuscated email addresses, add the Middleware class to the `$routeMiddleware` array in `App\Http\Middleware\Kernel.php`:

        protected $routeMiddleware = [
    		'guest' => 'App\Http\Middleware\RedirectIfAuthenticated',
    		...
    		'obfuscate' => 'Propaganistas\EmailObfuscator\Laravel\Middleware',
    	];

    and apply controller middleware as usual in a controller's construct method or route definition:

    ````php
    public function __construct()
    {
        $this->middleware('obfuscate');
    }
    ```

- If you want apply obfuscation on only specific strings, just use the `obfuscateEmail($string)` function.


#### Twig

Add the extension to the `Twig_Environment`:

```php
$twig = new Twig_Environment(...);
$twig->addExtension(new \Propaganistas\EmailObfuscator\Twig\Extension());
```

The extension exposes an `obfuscateEmail` Twig filter, which can be applied to any string.

```twig
{{ "Lorem Ipsum"|obfuscateEmail }}
{{ variable|obfuscateEmail }}
```

## Call for integrations

If you made an integration bundle/extension/package for a specific framework or CMS, please open a pull request.


## Credits

* [Scott Yang](http://scott.yang.id.au/2003/06/obfuscate-email-address-with-javascript-rot13) - For the Javascript used in this method.
* [Silvan Mühlemann](http://techblog.tilllate.com/2008/07/20/ten-methods-to-obfuscate-e-mail-addresses-compared/) - For the inspiration of the CSS implementation.
