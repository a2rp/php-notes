# 15. Security, errors, and production practices

[Back to notes index](../README.md)

| [Previous: Testing with PHPUnit](./14-testing-with-phpunit.md) | [Notes index](../README.md) | [Next: Build a small PHP JSON service](./16-build-a-php-json-service.md) |
| --- | --- | --- |

## Validate, authorize, and escape at the right boundary

Validate data when it enters the application. Check its type, length, format, and allowed range. Authentication answers who the caller is; authorization checks whether that caller may perform a specific action on a specific record.

Escape output for its destination. For HTML text and attributes, use **htmlspecialchars** with explicit flags and UTF-8. Use prepared statements for SQL values. Use a CSRF token for state-changing requests authenticated by a browser session.

Each protection handles a different risk. A valid email address can still contain text that needs HTML escaping. A prepared SQL statement does not authorize access to the row.

## Store passwords with password APIs

Do not store a plain-text password or a home-made hash. Use PHP's password functions:

~~~php
$hash = password_hash($password, PASSWORD_DEFAULT);

if (password_verify($submittedPassword, $hash)) {
    echo "Password matched";
}
~~~

Store the returned hash in a column large enough for future algorithm changes. After a successful login, rotate the session identifier and grant only the user access permitted by the application's authorization rules.

## Keep secrets and error details private

Read database credentials and service keys from protected configuration, not committed source files. Do not print them to logs or return them to a client.

In production, disable public display of detailed errors and send diagnostic information to a protected log. Return a safe message and suitable status code to the caller. In local development, enable useful error reporting so failures are easier to find.

Do not catch an error only to ignore it. Either recover deliberately, translate it at a clear application boundary, or allow it to reach the configured error handler.

## Limit resource use

Set limits for request sizes, uploaded files, database work, and long-running operations. Add pagination for growing result sets. Use streams for large files rather than reading the entire content into memory.

Avoid constructing operating-system commands from user input. If a process must be started, use a fixed executable and validated arguments. Keep uploaded files outside the public document root and never execute them as server code.

## Maintain dependencies and runtime settings

Use supported PHP and library releases. Commit the Composer manifest and lockfile, install the locked tree for builds, and review dependency advisories before changing versions. Confirm required PHP extensions in each deployment environment.

Configure the production web server and PHP process manager deliberately. Require HTTPS for sensitive traffic, use secure session cookie settings, restrict filesystem permissions, and keep debug interfaces private.

## Practice

Review the form, session, and PDO examples. Confirm that user data is validated, HTML output is escaped, SQL values are bound, CSRF tokens are checked, and errors do not expose secrets. Check that the production configuration hides displayed stack traces and that uploaded files are not directly executable.

## Check what you learned

1. What is the difference between authentication and authorization?
2. Why should HTML output be escaped even after validation?
3. Which PHP functions should store and verify a password?
4. Why should production errors be logged rather than displayed to users?
5. Why should secrets stay out of committed source files?
6. What resource limits protect a server from oversized work?
7. Why should uploaded files stay outside the public document root?
8. What should be checked before upgrading a dependency to address an advisory?

## References

- [PHP security](https://www.php.net/manual/en/security.php)
- [Password hashing](https://www.php.net/manual/en/book.password.php)
- [Cross-site scripting](https://www.php.net/manual/en/security.variables.php)
- [File upload security](https://www.php.net/manual/en/features.file-upload.php#features.file-upload.security)
- [Composer basic usage](https://getcomposer.org/doc/01-basic-usage.md)