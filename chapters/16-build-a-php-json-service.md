# 16. Build a small PHP JSON service

[Back to notes index](../README.md)

| [Previous: Security, errors, and production practices](./15-security-errors-and-production.md) | [Notes index](../README.md) | [Next: All code samples](./98-all-code-samples.md) |
| --- | --- | --- |

## What the service does

This small service exposes three routes:

- **GET /health** returns a status response.
- **GET /contacts** returns up to 100 saved contacts.
- **POST /contacts** validates and stores one contact.

It uses SQLite through PDO. The **pdo_sqlite** extension must be enabled. The database file is stored outside the public folder. This example is for local practice and does not include user authentication.

## Create the database and response helper

Create **public/index.php**:

~~~php
<?php

declare(strict_types=1);

header("Content-Type: application/json; charset=utf-8");

function sendJson(int $statusCode, array $data, array $headers = []): void
{
    http_response_code($statusCode);

    foreach ($headers as $name => $value) {
        header($name . ": " . $value);
    }

    echo json_encode($data, JSON_THROW_ON_ERROR);
}

$databaseDirectory = dirname(__DIR__) . "/var";

if (
    !is_dir($databaseDirectory) &&
    !mkdir($databaseDirectory, 0700, true) &&
    !is_dir($databaseDirectory)
) {
    sendJson(500, ["error" => "Service storage is unavailable"]);
    exit;
}

try {
    $pdo = new PDO("sqlite:" . $databaseDirectory . "/contacts.sqlite");
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $pdo->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);

    $pdo->exec(
        "CREATE TABLE IF NOT EXISTS contacts (
            id INTEGER PRIMARY KEY,
            name TEXT NOT NULL,
            email TEXT NOT NULL
        )"
    );

    $method = $_SERVER["REQUEST_METHOD"] ?? "GET";
    $path = parse_url($_SERVER["REQUEST_URI"] ?? "/", PHP_URL_PATH);

    if ($method === "GET" && $path === "/health") {
        sendJson(200, ["status" => "ok"]);
        exit;
    }

    if ($path !== "/contacts") {
        sendJson(404, ["error" => "Route not found"]);
        exit;
    }

    if ($method === "GET") {
        $statement = $pdo->query(
            "SELECT id, name, email FROM contacts ORDER BY id DESC LIMIT 100"
        );
        sendJson(200, ["contacts" => $statement->fetchAll()]);
        exit;
    }

    if ($method !== "POST") {
        sendJson(405, ["error" => "Method not allowed"], [
            "Allow" => "GET, POST",
        ]);
        exit;
    }

    $contentType = $_SERVER["CONTENT_TYPE"] ?? "";
    if (stripos($contentType, "application/json") !== 0) {
        sendJson(415, ["error" => "Send application/json"]);
        exit;
    }

    $maximumBytes = 65536;
    $bodyText = file_get_contents(
        "php://input",
        false,
        null,
        0,
        $maximumBytes + 1
    );

    if ($bodyText === false) {
        sendJson(400, ["error" => "Could not read request body"]);
        exit;
    }

    if (strlen($bodyText) > $maximumBytes) {
        sendJson(413, ["error" => "Request body is too large"]);
        exit;
    }

    $body = json_decode($bodyText, true, 512, JSON_THROW_ON_ERROR);
    $name = is_array($body) && is_string($body["name"] ?? null)
        ? trim($body["name"])
        : "";
    $email = is_array($body) && is_string($body["email"] ?? null)
        ? trim($body["email"])
        : "";

    if (
        $name === "" ||
        strlen($name) > 80 ||
        filter_var($email, FILTER_VALIDATE_EMAIL) === false
    ) {
        sendJson(400, ["error" => "Provide a name and valid email"]);
        exit;
    }

    $statement = $pdo->prepare(
        "INSERT INTO contacts (name, email) VALUES (:name, :email)"
    );
    $statement->execute([
        "name" => $name,
        "email" => $email,
    ]);

    sendJson(201, [
        "id" => (int) $pdo->lastInsertId(),
        "name" => $name,
        "email" => $email,
    ]);
} catch (JsonException $error) {
    sendJson(400, ["error" => "Request body must be valid JSON"]);
} catch (Throwable $error) {
    error_log("Contact service failed: " . $error->getMessage());
    sendJson(500, ["error" => "Service could not complete the request"]);
}
~~~

The insert uses a prepared statement. Request values are validated before storage, and unexpected details are written to the server log instead of returned to the client.

## Route requests through one entry point

Create **public/router.php**:

~~~php
<?php

require __DIR__ . "/index.php";
~~~

The built-in server uses this router for the practice service:

~~~sh
php -S 127.0.0.1:8000 -t public public/router.php
~~~

The service creates **var/contacts.sqlite** on its first request. Keep that folder writable by the local process and outside the public document root.

## Send a few requests

Check the health route:

~~~sh
curl -i http://127.0.0.1:8000/health
~~~

Create a contact:

~~~sh
curl -i -X POST http://127.0.0.1:8000/contacts -H "Content-Type: application/json" --data "{\"name\":\"Asha Rao\",\"email\":\"asha@example.test\"}"
~~~

List saved contacts:

~~~sh
curl -i http://127.0.0.1:8000/contacts
~~~

Try malformed JSON, a missing email, an unsupported content type, a large request body, and an unknown path. Check the status code and response for each case.

## Review the service boundary

The service checks the route, method, content type, body size, JSON shape, name, and email. PDO binds values separately from SQL. The database file stays outside the public folder.

Before adapting the example for real users, add authentication and record-level authorization, HTTPS, rate limits, structured logs, a migration process, and tests against a disposable database. Do not expose the development server as a production host.

## Check what you learned

1. Which routes does the service expose?
2. Why is the SQLite file outside the public directory?
3. What does the body-size limit protect?
4. Which status code reports an oversized request?
5. Why is the contact insert a prepared statement?
6. What does the Allow header tell a client?
7. Why does the service log unexpected errors but return a generic response?
8. Which protections are still needed before a real public deployment?

## References

- [PDO SQLite driver](https://www.php.net/manual/en/ref.pdo-sqlite.php)
- [PDO prepared statements](https://www.php.net/manual/en/pdo.prepared-statements.php)
- [JSON functions](https://www.php.net/manual/en/ref.json.php)
- [PHP built-in web server](https://www.php.net/manual/en/features.commandline.webserver.php)
- [PHP security](https://www.php.net/manual/en/security.php)