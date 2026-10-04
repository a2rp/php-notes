# 11. Validation, sessions, cookies, and CSRF

[Back to notes index](../README.md)

| [Previous: HTTP requests, responses, and forms](./10-http-requests-responses-and-forms.md) | [Notes index](../README.md) | [Next: PDO, prepared statements, and transactions](./12-pdo-prepared-statements-and-transactions.md) |
| --- | --- | --- |

## Validate input on the server

Browser validation helps a person fill out a form, but a client can send any request. Validate values on the server for the expected type, length, format, and allowed range:

~~~php
$emailValue = $_POST["email"] ?? "";
$email = is_string($emailValue) ? trim($emailValue) : "";

if (filter_var($email, FILTER_VALIDATE_EMAIL) === false) {
    http_response_code(400);
    echo "Enter a valid email address";
    exit;
}
~~~

Validation checks whether a value is acceptable for the application's rules. Escaping serves a different purpose: it makes a value safe for a particular output context.

## Start and configure a session

A session stores server-side state associated with a session identifier sent by the browser. Configure the cookie before starting the session, and start it before sending output:

~~~php
<?php

session_set_cookie_params([
    "secure" => true,
    "httponly" => true,
    "samesite" => "Lax",
]);

session_start();
~~~

The **secure** option assumes HTTPS. Configure it appropriately for the local environment and require HTTPS for a deployed application. **httponly** prevents ordinary client-side scripts from reading the session cookie. **samesite** helps limit cross-site cookie sending.

After successful authentication, rotate the session identifier:

~~~php
session_regenerate_id(true);
$_SESSION["user_id"] = $userId;
~~~

Store only necessary state in the session. Do not trust arbitrary values a client places in a cookie.

## Add a CSRF token to a form

Cross-site request forgery can cause a browser to send an authenticated request the user did not intend. A session-bound token can show that a form was generated for the current session:

~~~php
if (!isset($_SESSION["csrf_token"])) {
    $_SESSION["csrf_token"] = bin2hex(random_bytes(32));
}

$csrfToken = $_SESSION["csrf_token"];
~~~

Put the token in a hidden field and escape it in the HTML form:

~~~php
<input
  type="hidden"
  name="csrf_token"
  value="<?= htmlspecialchars($csrfToken, ENT_QUOTES | ENT_SUBSTITUTE, "UTF-8") ?>"
>
~~~

Check the submitted token before changing state:

~~~php
$submittedToken = $_POST["csrf_token"] ?? "";

if (
    !is_string($submittedToken) ||
    !hash_equals($_SESSION["csrf_token"] ?? "", $submittedToken)
) {
    http_response_code(403);
    exit("Invalid request token");
}
~~~

Generate tokens with **random_bytes** and compare them with **hash_equals**. Do not use a predictable timestamp or the session ID itself as the token.

## Tell CSRF and XSS apart

CSRF abuses a browser's existing authenticated state to send an unwanted request. Cross-site scripting injects executable content into a page viewed by another user. A CSRF token does not replace HTML output escaping, and HTML escaping does not prove that a state-changing request was intended.

Use protections together: validate input, escape output for its context, use CSRF defenses for state changes, and keep authentication and authorization checks on the server.

## Practice

Validate an email field on the server. Start a session before output, add a CSRF token to a form, and reject a missing or changed token. Confirm that authentication rotates the session ID and that the session cookie is configured for the deployment.

## Check what you learned

1. Why is browser-side form validation insufficient?
2. What does filter_var with FILTER_VALIDATE_EMAIL check?
3. Where is session state stored?
4. When should session_set_cookie_params be called?
5. Why rotate the session identifier after authentication?
6. What does a CSRF token help prove?
7. Why use hash_equals to compare a submitted token?
8. How does CSRF differ from cross-site scripting?

## References

- [Filter functions](https://www.php.net/manual/en/book.filter.php)
- [Sessions](https://www.php.net/manual/en/book.session.php)
- [Session cookie parameters](https://www.php.net/manual/en/function.session-set-cookie-params.php)
- [random_bytes](https://www.php.net/manual/en/function.random-bytes.php)
- [hash_equals](https://www.php.net/manual/en/function.hash-equals.php)
- [PHP security](https://www.php.net/manual/en/security.php)