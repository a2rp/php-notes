# 2. PHP CLI, built-in server, and configuration

[Back to notes index](../README.md)

| [Previous: PHP runtime and request lifecycle](./01-php-runtime-and-request-lifecycle.md) | [Notes index](../README.md) | [Next: Values, variables, and type declarations](./03-values-variables-and-types.md) |
| --- | --- | --- |

## Check the command-line runtime

Install a supported PHP release for the operating system, then open a new terminal and check the interpreter:

~~~sh
php --version
php --help
~~~

If the shell cannot find **php**, check whether the PHP installation directory is on PATH and open a fresh terminal.

Inspect the configuration file and loaded extensions:

~~~sh
php --ini
php -m
~~~

The CLI and web server can load different configuration files. When a feature works in one environment but not the other, compare the actual runtime and extension configuration for both.

## Check an extension

A PHP extension adds functionality such as database drivers, image operations, or internationalization. Check whether a named extension is loaded:

~~~sh
php --ri pdo_sqlite
~~~

The result depends on how PHP was installed and configured. Do not assume an extension exists because it is present on another developer's machine. Document required extensions in the project setup.

## Validate syntax before running

Ask PHP to parse a file without executing it:

~~~sh
php -l app.php
~~~

Use this after changing PHP code to catch parse errors early. It does not prove the behavior is correct, so run relevant examples and tests too.

## Serve a local project

For a small local project with a **public** document root, start PHP's built-in development server:

~~~sh
php -S 127.0.0.1:8000 -t public
~~~

Open **http://127.0.0.1:8000** in a browser. The option **-t public** makes the public folder the document root. Keep application source, configuration, and local secrets outside that folder unless the router is designed to protect them.

The built-in server is for development and controlled testing. Use a production web server and PHP process manager for a deployed application.

## Configure the runtime deliberately

PHP settings can come from **php.ini**, server configuration, or the process environment. Check values in the environment where the script actually runs. A command-line change does not necessarily alter the web server configuration.

Development should show useful errors locally. Production should log errors to a protected destination and avoid returning stack traces or private paths to users. Do not publish a diagnostic page to inspect production settings.

## Practice

Run a PHP script from the CLI, list enabled extensions, and lint it. Start the built-in server with a temporary **public/index.php** file. Compare its version and loaded extensions with the CLI, then stop the server with Ctrl+C.

## Check what you learned

1. Which command displays the PHP runtime version?
2. Which command shows the loaded CLI configuration file?
3. What does a PHP extension provide?
4. What does php -l check?
5. What does -t public select for the built-in server?
6. Why can CLI and web configurations differ?
7. Is the built-in PHP server intended for production deployment?
8. Where should production errors be logged?

## References

- [PHP command-line options](https://www.php.net/manual/en/features.commandline.options.php)
- [PHP configuration file](https://www.php.net/manual/en/configuration.file.php)
- [PHP extensions](https://www.php.net/manual/en/extensions.php)
- [Built-in web server](https://www.php.net/manual/en/features.commandline.webserver.php)