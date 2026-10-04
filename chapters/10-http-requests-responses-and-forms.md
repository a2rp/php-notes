# 10. HTTP requests, responses, and forms

[Back to notes index](../README.md)

| [Previous: Namespaces, Composer, and autoloading](./09-namespaces-composer-and-autoloading.md) | [Notes index](../README.md) | [Next: Validation, sessions, cookies, and CSRF](./11-validation-sessions-cookies-and-csrf.md) |
| --- | --- | --- |

## Read the request method

PHP exposes the HTTP method through **$_SERVER["REQUEST_METHOD"]**. Query-string values are available in **$_GET** and form values sent with POST are available in **$_POST**.

~~~php
<?php

$method = $_SERVER["REQUEST_METHOD"] ?? "GET";

if ($method === "POST") {
    $nameValue = $_POST["name"] ?? "";
    $name = is_string($nameValue) ? trim($nameValue) : "";

    if ($name === "") {
        http_response_code(400);
        echo "A name is required";
        exit;
    }

    echo "Received: " . $name;
    exit;
}

$nameValue = $_GET["name"] ?? "";
$name = is_string($nameValue) ? trim($nameValue) : "";

echo "Hello, " . $name;
~~~

Values from GET and POST are controlled by the client. They can be absent or have a different shape than expected. Do not assume every value is a string.

Avoid **$_REQUEST** when the source matters. It can combine request sources according to configuration, making it harder to know where a value came from.

## Create a form

An HTML form names its fields and declares how the browser sends them:

~~~html
<form method="post" action="/contact.php">
  <label>
    Name
    <input name="name" required maxlength="80">
  </label>

  <button type="submit">Send</button>
</form>
~~~

The **name** attribute is the key PHP receives in **$_POST**. Browser-side required fields improve usability, but server-side validation is still necessary because a client can send a request without using the page.

## Set response headers before output

Set a response's content type and status before writing the body:

~~~php
<?php

header("Content-Type: text/html; charset=utf-8");
http_response_code(200);
~~~

If output has already been sent, PHP may no longer be able to change headers. Keep whitespace and debug output out of files that set response headers.

## Escape values for HTML output

Text received from a user must be escaped for the context where it is inserted. For HTML text and quoted attributes, use **htmlspecialchars** with an explicit encoding:

~~~php
<?php

$name = $_POST["name"] ?? "";
$name = is_string($name) ? trim($name) : "";

echo "<p>Hello, ";
echo htmlspecialchars($name, ENT_QUOTES | ENT_SUBSTITUTE, "UTF-8");
echo "</p>";
~~~

Encoding for HTML is not the same as validation. Validate a value for its intended meaning, then escape it when outputting it into HTML. JavaScript, CSS, URLs, and SQL have different context rules.

## Return an error status

Use an HTTP status code that matches the result. For example, an invalid form can return **400 Bad Request**, while an authenticated user who lacks permission may need **403 Forbidden**. Keep user-facing errors clear and avoid showing internal paths or stack traces.

## Practice

Serve a form through the local PHP server. Submit a valid name, an empty name, and an array-shaped parameter. Confirm the server checks the value and renders it with HTML escaping.

## Check what you learned

1. Which superglobal contains the request method?
2. Where do GET query values appear?
3. Where do POST form values appear?
4. Why should $_REQUEST be avoided when the source matters?
5. What does an input's name attribute control?
6. Why is browser-side required validation insufficient?
7. When should response headers be set?
8. Why should untrusted text be escaped at the HTML output point?

## References

- [Handling external variables](https://www.php.net/manual/en/language.variables.external.php)
- [$_GET](https://www.php.net/manual/en/reserved.variables.get.php)
- [$_POST](https://www.php.net/manual/en/reserved.variables.post.php)
- [header](https://www.php.net/manual/en/function.header.php)
- [htmlspecialchars](https://www.php.net/manual/en/function.htmlspecialchars.php)