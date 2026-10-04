# 9. Namespaces, Composer, and autoloading

[Back to notes index](../README.md)

| [Previous: Interfaces, traits, enums, and exceptions](./08-interfaces-traits-enums-and-exceptions.md) | [Notes index](../README.md) | [Next: HTTP requests, responses, and forms](./10-http-requests-responses-and-forms.md) |
| --- | --- | --- |

## Group class names with namespaces

A namespace groups related names and prevents class-name collisions:

~~~php
<?php

namespace App\Service;

final class Greeting
{
    public function message(string $name): string
    {
        return "Hello, " . $name;
    }
}
~~~

The fully qualified class name is **App\Service\Greeting**. Use imports to make it shorter in another file:

~~~php
<?php

namespace App\Controller;

use App\Service\Greeting;

final class HomeController
{
    public function __construct(
        private Greeting $greeting
    ) {
    }
}
~~~

Place a namespace declaration near the top of the file, after the opening tag and any declare statement.

## Describe project dependencies with Composer

Composer manages PHP packages and generates an autoloader. Start a project manifest with:

~~~sh
composer init
~~~

A project **composer.json** can declare the PHP version, package dependencies, and namespace mapping:

~~~json
{
  "name": "a2rp/php-study-example",
  "type": "project",
  "autoload": {
    "psr-4": {
      "App\\": "src/"
    }
  }
}
~~~

The **App\\** prefix maps classes such as **App\Service\Greeting** to files under **src/**. Class names and file paths should follow the PSR-4 mapping convention.

## Load project classes automatically

After changing the autoload mapping, regenerate Composer's autoloader:

~~~sh
composer dump-autoload
~~~

An application loads it near the entry point:

~~~php
<?php

require __DIR__ . "/vendor/autoload.php";

$greeting = new App\Service\Greeting();
echo $greeting->message("Ashish");
~~~

Composer creates **vendor/autoload.php**. Do not edit generated files in **vendor/** directly. Add dependencies with Composer and let the autoloader find their classes.

## Keep dependency installs reproducible

Composer uses **composer.json** to express package constraints and **composer.lock** to record the resolved versions for an application. Commit both files. After obtaining a project, run **composer install** to install the locked versions.

Do not commit **vendor/** for an ordinary application. It can be rebuilt from the manifest and lockfile. Check required PHP extensions and runtime constraints when a package fails to install.

## Practice

Create a project with **composer init**. Add an **App\\** PSR-4 mapping to **src/**, generate the autoloader, and create a class under **src/Service**. Load Composer's autoloader from a small entry point and instantiate the class.

## Check what you learned

1. What problem does a namespace help prevent?
2. What is the fully qualified name for App\Service\Greeting?
3. What does Composer manage?
4. What does a PSR-4 mapping connect?
5. When should composer dump-autoload be run?
6. What is vendor/autoload.php used for?
7. What information does composer.lock record?
8. Why should generated files in vendor not be edited directly?

## References

- [Namespaces](https://www.php.net/manual/en/language.namespaces.php)
- [Composer basic usage](https://getcomposer.org/doc/01-basic-usage.md)
- [Composer autoload schema](https://getcomposer.org/doc/04-schema.md#autoload)
- [Composer lock file](https://getcomposer.org/doc/01-basic-usage.md#installing-dependencies)