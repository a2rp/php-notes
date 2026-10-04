# 1. PHP runtime and request lifecycle

[Back to notes index](../README.md)

| [Previous: Notes index](../README.md) | [Notes index](../README.md) | [Next: PHP CLI, built-in server, and configuration](./02-php-cli-server-and-configuration.md) |
| --- | --- | --- |

## What PHP does

PHP is a programming language and runtime commonly used for server-side web applications. The PHP interpreter can also run scripts from a command line, process scheduled jobs, and generate other output.

A PHP file contains source code that the runtime parses and executes. The runtime supplies built-in functions, data types, extensions, and interfaces to the operating system or web server.

Create **hello.php**:

~~~php
<?php

echo "Hello from PHP\n";
echo "PHP version: " . PHP_VERSION . "\n";
echo "Runtime interface: " . PHP_SAPI . "\n";
~~~

Run it from a terminal:

~~~sh
php hello.php
~~~

The **PHP_VERSION** constant reports the runtime version. **PHP_SAPI** reports the server API through which PHP is running, such as the command-line interface or a web server interface.

## Run PHP from the command line

A CLI script can read command-line arguments from **$argv**:

~~~php
<?php

$name = $argv[1] ?? "friend";
echo "Hello, " . $name . "\n";
~~~

Run it with:

~~~sh
php greet.php Ashish
~~~

Arguments are strings. Validate and convert them before using them in calculations, file paths, or commands.

## Understand a web request

For a web request, a web server or PHP process receives the request, selects the PHP entry point, and executes the code. The script can read request information, call application code, and send a response. In a common PHP-FPM setup, the web server forwards PHP work to a pool of PHP worker processes.

A request path is not the same as a filesystem path. The server configuration maps public URLs to application routes or entry files. Keep private configuration and source files outside the web document root when possible.

PHP-FPM may reuse worker processes, but application code should not rely on a request's mutable state being carried into a later request. Use explicit storage such as a database or session mechanism for state that must persist.

## Read request context carefully

PHP exposes request details through superglobals such as **$_GET**, **$_POST**, **$_SERVER**, and **$_COOKIE**. These values can be missing or controlled by a client. Treat them as untrusted input and validate them before using them.

A web response is sent by writing output and setting response headers. Output written before a header can prevent the application from changing that header. Later chapters show how to keep request handling and response generation organized.

## Keep development information private

The **phpinfo()** function displays extensive runtime and configuration details. It can help in a local environment, but exposing it publicly can reveal paths, extensions, and settings. Remove diagnostic pages from publicly accessible deployments.

Use the official manual for the runtime and extensions installed on the machine. Examples in these notes focus on the language and built-in APIs, while configuration may differ by operating system and server.

## Practice

Run **hello.php** from the terminal and compare **PHP_SAPI** with the value shown from a local development server. Run **greet.php** with and without an argument, then add a check that rejects an empty name.

## Check what you learned

1. What is PHP used for besides serving web pages?
2. Which runtime constant reports the PHP version?
3. What does PHP_SAPI report?
4. Which variable contains command-line arguments?
5. Why should command-line arguments be validated?
6. What role can PHP-FPM play in a web request?
7. Why should an application avoid relying on mutable worker state between requests?
8. Why should phpinfo output not be exposed publicly?

## References

- [PHP introduction](https://www.php.net/manual/en/introduction.php)
- [PHP command-line usage](https://www.php.net/manual/en/features.commandline.php)
- [PHP-FPM](https://www.php.net/manual/en/install.fpm.php)
- [Predefined variables](https://www.php.net/manual/en/language.variables.predefined.php)