## Laravel IDE Helper Generator

### Complete phpDocs, directly from the source

This packages generates a file that your IDE can understand, so it can provide accurate autocompletion. Generation is done, based on the files in your project, so they are alway up-to-date.
If you don't want to generate it, you can add a pre-generated file to the root folder of your laravel project. (But this isn't as up-to-date as self generated files)

* Generated version: https://gist.github.com/barryvdh/5227822

Note: You do need CodeIntel for Sublime Text: https://github.com/Kronuz/SublimeCodeIntel

### Automatic phpDoc generation for Laravel Facades

Require this package in your composer.json and run composer update (or run `composer require barryvdh/laravel-ide-helper:1.*` directly):

    "barryvdh/laravel-ide-helper": "1.*"

After updating composer, add the ServiceProvider to the providers array in app/config/app.php

    'Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider',

You can now re-generate the docs yourself (for future updates) in artisan

    php artisan ide-helper:generate

Note: bootstrap/compiled.php has to be cleared first, so run `php artisan clear-compiled` before generating (and `php artisan optimize` after..)

You can configure your composer.json to do this after each commit:

    "scripts":{
        "post-update-cmd":[
            "php artisan ide-helper:generate",
            "php artisan optimize",
        ]
    },

You can also publish the config-file to change implementations (ie. interface to specific class) or set defaults for --helpers or --sublime.

    php artisan config:publish barryvdh/laravel-ide-helper

The generator tries to identify the real class, but if it cannot be found, you can define it in the config file.

Some classes need a working database connection. If you do not have a working default connection, some facades will not be included.
You can use a in-memory sqlite driver, using the -M option.

You can choose to include helper files. This is not enabled by default, but you can override this with the --helpers (-H) option.
The Illuminate/Support/helpers.php is already set-up, but you can add/remove your own files in the config file.


### Automatic phpDocs for Models

If you don't want to write your properties yourself, you can use the command `ide-helper:models` to generate
phpDocs, based on table columns, relations and getters/setters. Still in beta, so please provide feedback if you want.
Docs are written to a phpfile (_ide_helper_models.php) in the root of the project, so you can move the docs to the real model.

You can now also write the comments directly to your Model file, using the -W argument. Please make sure to backup your models, before writing the info.
It should keep the existing comments and only append new properties/methods. The existing phpdoc is replaced, or added if not found.

For now, only models in app/models are scanned. The optional argument tells what models to use (also outside app/models).
`php artisan ide-helper:models Post,User`

Note: With namespaces, uses \\ instead of \
`php artisan ide-helper:models API\\User`

This creates a file with the phpDocs for each Model. You should check and change them to be more accurate.
Also, all relations are Eloquent|Eloquent[] by default, you can change them to the actual Model.
After copying the phpdocs to your model, you can clear the file, so your IDE only uses the real source.


### License

The Laravel IDE Helper Generator is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
