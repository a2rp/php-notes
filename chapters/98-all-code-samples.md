# All code samples

[Back to notes index](../README.md)

| [Previous: Build a small PHP JSON service](./16-build-a-php-json-service.md) | [Notes index](../README.md) | [Next: Complete questions and answers](./99-complete-q-and-a.md) |
| --- | --- | --- |

This appendix gathers the examples from the core chapters in chapter order. Return to each chapter for explanations and setup notes around a sample.

## 1. PHP runtime and request lifecycle

### Sample 1 (php)

~~~~php
<?php

echo "Hello from PHP\n";
echo "PHP version: " . PHP_VERSION . "\n";
echo "Runtime interface: " . PHP_SAPI . "\n";
~~~~

### Sample 2 (sh)

~~~~sh
php hello.php
~~~~

### Sample 3 (php)

~~~~php
<?php

$name = $argv[1] ?? "friend";
echo "Hello, " . $name . "\n";
~~~~

### Sample 4 (sh)

~~~~sh
php greet.php Ashish
~~~~

## 2. PHP CLI, built-in server, and configuration

### Sample 5 (sh)

~~~~sh
php --version
php --help
~~~~

### Sample 6 (sh)

~~~~sh
php --ini
php -m
~~~~

### Sample 7 (sh)

~~~~sh
php --ri pdo_sqlite
~~~~

### Sample 8 (sh)

~~~~sh
php -l app.php
~~~~

### Sample 9 (sh)

~~~~sh
php -S 127.0.0.1:8000 -t public
~~~~

## 3. Values, variables, and type declarations

### Sample 10 (php)

~~~~php
<?php

$name = "Ashish";
$years = 27;
$isLearning = true;
$score = null;

var_dump($name, $years, $isLearning, $score);
~~~~

### Sample 11 (php)

~~~~php
$value = "42";
echo get_debug_type($value);
~~~~

### Sample 12 (php)

~~~~php
$input = "0";

var_dump($input == 0);
var_dump($input === 0);
~~~~

### Sample 13 (php)

~~~~php
<?php

declare(strict_types=1);

function add(int $left, int $right): int
{
    return $left + $right;
}

echo add(2, 3);
~~~~

### Sample 14 (php)

~~~~php
function displayName(?string $name): string
{
    return $name ?? "Guest";
}

function formatIdentifier(int|string $identifier): string
{
    return (string) $identifier;
}
~~~~

### Sample 15 (php)

~~~~php
$person = [
    "name" => "Asha",
    "active" => true,
    "visits" => 3,
];

var_dump($person);
~~~~

## 4. Operators, conditions, and loops

### Sample 16 (php)

~~~~php
$subtotal = 125.50;
$taxRate = 0.08;
$total = $subtotal + ($subtotal * $taxRate);

$isLargeOrder = $total >= 100;
echo $total;
var_dump($isLargeOrder);
~~~~

### Sample 17 (php)

~~~~php
$age = 22;
$hasPermission = true;

if ($age >= 18 && $hasPermission) {
    echo "Access allowed";
} else {
    echo "Access denied";
}
~~~~

### Sample 18 (php)

~~~~php
$score = 84;

if ($score >= 90) {
    $grade = "A";
} elseif ($score >= 75) {
    $grade = "B";
} else {
    $grade = "Keep practicing";
}

echo $grade;
~~~~

### Sample 19 (php)

~~~~php
$method = "POST";

switch ($method) {
    case "GET":
        echo "Read data";
        break;
    case "POST":
        echo "Create data";
        break;
    default:
        echo "Unsupported method";
}
~~~~

### Sample 20 (php)

~~~~php
for ($number = 1; $number <= 3; $number++) {
    echo "Item " . $number . PHP_EOL;
}
~~~~

### Sample 21 (php)

~~~~php
$attempts = 0;

while ($attempts < 3) {
    echo "Attempt " . $attempts . PHP_EOL;
    $attempts++;
}
~~~~

### Sample 22 (php)

~~~~php
for ($number = 1; $number <= 5; $number++) {
    if ($number === 2) {
        continue;
    }

    if ($number === 4) {
        break;
    }

    echo $number . PHP_EOL;
}
~~~~

## 5. Functions, scope, and closures

### Sample 23 (php)

~~~~php
<?php

declare(strict_types=1);

function totalWithTax(float $amount, float $rate): float
{
    return $amount + ($amount * $rate);
}

echo totalWithTax(100.0, 0.08);
~~~~

### Sample 24 (php)

~~~~php
function greet(string $name = "friend"): string
{
    return "Hello, " . $name;
}

echo greet();
echo greet("Asha");
~~~~

### Sample 25 (php)

~~~~php
function displayLabel(?string $label): string
{
    return $label ?? "Untitled";
}
~~~~

### Sample 26 (php)

~~~~php
function makeMessage(string $name): string
{
    $greeting = "Hello";
    return $greeting . ", " . $name;
}

echo makeMessage("Ravi");
~~~~

### Sample 27 (php)

~~~~php
$taxRate = 0.08;

$addTax = function (float $amount) use ($taxRate): float {
    return $amount + ($amount * $taxRate);
};

echo $addTax(100.0);
~~~~

### Sample 28 (php)

~~~~php
$factor = 3;
$multiply = fn (int $value): int => $value * $factor;

echo $multiply(4);
~~~~

### Sample 29 (php)

~~~~php
function percentageOf(float $amount, float $percent): float
{
    if ($amount < 0 || $percent < 0 || $percent > 100) {
        throw new InvalidArgumentException("Values are outside the allowed range");
    }

    return $amount * ($percent / 100);
}
~~~~

## 6. Arrays, strings, and JSON

### Sample 30 (php)

~~~~php
$prices = [12.50, 8.00, 4.25];

foreach ($prices as $price) {
    echo $price . PHP_EOL;
}

echo array_sum($prices);
~~~~

### Sample 31 (php)

~~~~php
$employee = [
    "id" => 201,
    "name" => "Asha Rao",
    "active" => true,
];

echo $employee["name"];
~~~~

### Sample 32 (php)

~~~~php
$department = $employee["department"] ?? "Unassigned";
~~~~

### Sample 33 (php)

~~~~php
$numbers = [2, 5, 8, 11];

$doubled = array_map(
    fn (int $number): int => $number * 2,
    $numbers
);

$largeValues = array_filter(
    $numbers,
    fn (int $number): bool => $number >= 8
);
~~~~

### Sample 34 (php)

~~~~php
$rawName = "  Asha Rao  ";
$name = trim($rawName);

if (str_contains($name, " ")) {
    echo str_replace(" ", "-", $name);
}
~~~~

### Sample 35 (php)

~~~~php
$payload = [
    "id" => 201,
    "name" => "Asha Rao",
];

$json = json_encode($payload, JSON_THROW_ON_ERROR);
echo $json;
~~~~

### Sample 36 (php)

~~~~php
$decoded = json_decode(
    '{"active":true,"visits":3}',
    true,
    512,
    JSON_THROW_ON_ERROR
);

var_dump($decoded["active"], $decoded["visits"]);
~~~~

## 7. Classes, objects, and properties

### Sample 37 (php)

~~~~php
<?php

declare(strict_types=1);

final class Money
{
    private int $cents;

    public function __construct(int $cents)
    {
        if ($cents < 0) {
            throw new InvalidArgumentException("Money cannot be negative");
        }

        $this->cents = $cents;
    }

    public function cents(): int
    {
        return $this->cents;
    }

    public function format(): string
    {
        return number_format($this->cents / 100, 2);
    }
}

$price = new Money(1250);
echo $price->format();
~~~~

### Sample 38 (php)

~~~~php
class Counter
{
    private int $value = 0;

    public function increment(): void
    {
        $this->value++;
    }

    public function value(): int
    {
        return $this->value;
    }
}
~~~~

### Sample 39 (php)

~~~~php
final class Order
{
    public function __construct(
        private Money $total
    ) {
    }

    public function total(): Money
    {
        return $this->total;
    }
}
~~~~

## 8. Interfaces, traits, enums, and exceptions

### Sample 40 (php)

~~~~php
interface Notifier
{
    public function send(string $recipient, string $message): void;
}

final class EmailNotifier implements Notifier
{
    public function send(string $recipient, string $message): void
    {
        echo "Sending to " . $recipient . ": " . $message;
    }
}
~~~~

### Sample 41 (php)

~~~~php
trait HasCreatedAt
{
    private DateTimeImmutable $createdAt;

    public function createdAt(): DateTimeImmutable
    {
        return $this->createdAt;
    }

    private function markCreated(): void
    {
        $this->createdAt = new DateTimeImmutable();
    }
}

final class Article
{
    use HasCreatedAt;

    public function __construct()
    {
        $this->markCreated();
    }
}
~~~~

### Sample 42 (php)

~~~~php
enum OrderStatus: string
{
    case Draft = "draft";
    case Paid = "paid";
    case Shipped = "shipped";
}

$status = OrderStatus::Paid;
echo $status->value;
~~~~

### Sample 43 (php)

~~~~php
function requirePositive(int $value): int
{
    if ($value <= 0) {
        throw new InvalidArgumentException("Value must be positive");
    }

    return $value;
}

try {
    echo requirePositive(-1);
} catch (InvalidArgumentException $error) {
    echo $error->getMessage();
}
~~~~

### Sample 44 (php)

~~~~php
final class MissingOrder extends RuntimeException
{
}
~~~~

## 9. Namespaces, Composer, and autoloading

### Sample 45 (php)

~~~~php
<?php

namespace App\Service;

final class Greeting
{
    public function message(string $name): string
    {
        return "Hello, " . $name;
    }
}
~~~~

### Sample 46 (php)

~~~~php
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
~~~~

### Sample 47 (sh)

~~~~sh
composer init
~~~~

### Sample 48 (json)

~~~~json
{
  "name": "a2rp/php-study-example",
  "type": "project",
  "autoload": {
    "psr-4": {
      "App\\": "src/"
    }
  }
}
~~~~

### Sample 49 (sh)

~~~~sh
composer dump-autoload
~~~~

### Sample 50 (php)

~~~~php
<?php

require __DIR__ . "/vendor/autoload.php";

$greeting = new App\Service\Greeting();
echo $greeting->message("Ashish");
~~~~

## 10. HTTP requests, responses, and forms

### Sample 51 (php)

~~~~php
<?php

header("Content-Type: text/plain; charset=utf-8");

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
~~~~

### Sample 52 (html)

~~~~html
<form method="post" action="/contact.php">
  <label>
    Name
    <input name="name" required maxlength="80">
  </label>

  <button type="submit">Send</button>
</form>
~~~~

### Sample 53 (php)

~~~~php
<?php

header("Content-Type: text/html; charset=utf-8");
http_response_code(200);
~~~~

### Sample 54 (php)

~~~~php
<?php

$name = $_POST["name"] ?? "";
$name = is_string($name) ? trim($name) : "";

echo "<p>Hello, ";
echo htmlspecialchars($name, ENT_QUOTES | ENT_SUBSTITUTE, "UTF-8");
echo "</p>";
~~~~

## 11. Validation, sessions, cookies, and CSRF

### Sample 55 (php)

~~~~php
$emailValue = $_POST["email"] ?? "";
$email = is_string($emailValue) ? trim($emailValue) : "";

if (filter_var($email, FILTER_VALIDATE_EMAIL) === false) {
    http_response_code(400);
    echo "Enter a valid email address";
    exit;
}
~~~~

### Sample 56 (php)

~~~~php
<?php

session_set_cookie_params([
    "secure" => getenv("APP_HTTPS") === "1",
    "httponly" => true,
    "samesite" => "Lax",
]);

session_start();
~~~~

### Sample 57 (php)

~~~~php
session_regenerate_id(true);
$_SESSION["user_id"] = $userId;
~~~~

### Sample 58 (php)

~~~~php
if (!isset($_SESSION["csrf_token"])) {
    $_SESSION["csrf_token"] = bin2hex(random_bytes(32));
}

$csrfToken = $_SESSION["csrf_token"];
~~~~

### Sample 59 (php)

~~~~php
<input
  type="hidden"
  name="csrf_token"
  value="<?= htmlspecialchars($csrfToken, ENT_QUOTES | ENT_SUBSTITUTE, "UTF-8") ?>"
>
~~~~

### Sample 60 (php)

~~~~php
$submittedToken = $_POST["csrf_token"] ?? "";

if (
    !is_string($submittedToken) ||
    !hash_equals($_SESSION["csrf_token"] ?? "", $submittedToken)
) {
    http_response_code(403);
    exit("Invalid request token");
}
~~~~

## 12. PDO, prepared statements, and transactions

### Sample 61 (php)

~~~~php
<?php

$pdo = new PDO("sqlite:" . __DIR__ . "/practice.sqlite");
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
$pdo->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);

$pdo->exec(
    "CREATE TABLE IF NOT EXISTS contacts (
        id INTEGER PRIMARY KEY,
        name TEXT NOT NULL,
        email TEXT NOT NULL
    )"
);
~~~~

### Sample 62 (php)

~~~~php
$statement = $pdo->prepare(
    "INSERT INTO contacts (name, email) VALUES (:name, :email)"
);

$statement->execute([
    "name" => "Asha Rao",
    "email" => "asha@example.test",
]);
~~~~

### Sample 63 (php)

~~~~php
$statement = $pdo->prepare(
    "SELECT id, name, email FROM contacts WHERE email = :email"
);

$statement->execute(["email" => "asha@example.test"]);
$contact = $statement->fetch();

if ($contact === false) {
    echo "No contact found";
} else {
    echo $contact["name"];
}
~~~~

### Sample 64 (php)

~~~~php
try {
    $pdo->beginTransaction();

    $statement = $pdo->prepare(
        "UPDATE contacts SET name = :name WHERE id = :id"
    );
    $statement->execute([
        "name" => "Asha R.",
        "id" => 1,
    ]);

    $pdo->commit();
} catch (Throwable $error) {
    if ($pdo->inTransaction()) {
        $pdo->rollBack();
    }

    throw $error;
}
~~~~

## 13. Files, uploads, and streams

### Sample 65 (php)

~~~~php
<?php

$path = __DIR__ . "/data/message.txt";

if (!is_file($path) || !is_readable($path)) {
    throw new RuntimeException("Practice file is unavailable");
}

$message = file_get_contents($path);
echo $message;
~~~~

### Sample 66 (php)

~~~~php
<?php

$file = $_FILES["document"] ?? null;

if (!is_array($file) || $file["error"] !== UPLOAD_ERR_OK) {
    http_response_code(400);
    exit("Upload failed");
}

$maximumBytes = 2 * 1024 * 1024;

if ($file["size"] > $maximumBytes || !is_uploaded_file($file["tmp_name"])) {
    http_response_code(400);
    exit("File is not accepted");
}

$mimeType = (new finfo(FILEINFO_MIME_TYPE))->file($file["tmp_name"]);
$allowedTypes = [
    "application/pdf" => "pdf",
    "image/png" => "png",
];

if (!isset($allowedTypes[$mimeType])) {
    http_response_code(415);
    exit("Unsupported file type");
}
~~~~

### Sample 67 (php)

~~~~php
$storageDirectory = dirname(__DIR__) . "/private-uploads";

if (
    !is_dir($storageDirectory) &&
    !mkdir($storageDirectory, 0700, true) &&
    !is_dir($storageDirectory)
) {
    throw new RuntimeException("Could not create upload storage");
}

$storedName = bin2hex(random_bytes(16)) . "." . $allowedTypes[$mimeType];
$destination = $storageDirectory . "/" . $storedName;

if (!move_uploaded_file($file["tmp_name"], $destination)) {
    throw new RuntimeException("Could not store uploaded file");
}
~~~~

### Sample 68 (php)

~~~~php
$handle = fopen($path, "rb");

if ($handle === false) {
    throw new RuntimeException("Could not open file");
}

try {
    while (!feof($handle)) {
        $chunk = fread($handle, 8192);

        if ($chunk === false) {
            throw new RuntimeException("Could not read file");
        }

        processChunk($chunk);
    }
} finally {
    fclose($handle);
}
~~~~

## 14. Testing with PHPUnit

### Sample 69 (sh)

~~~~sh
composer require --dev phpunit/phpunit
~~~~

### Sample 70 (php)

~~~~php
<?php

declare(strict_types=1);

namespace App\Billing;

use InvalidArgumentException;

final class PriceCalculator
{
    public function total(int $amountInCents, int $taxInCents): int
    {
        if ($amountInCents < 0 || $taxInCents < 0) {
            throw new InvalidArgumentException("Amounts cannot be negative");
        }

        return $amountInCents + $taxInCents;
    }
}
~~~~

### Sample 71 (php)

~~~~php
<?php

declare(strict_types=1);

namespace App\Tests\Billing;

use App\Billing\PriceCalculator;
use InvalidArgumentException;
use PHPUnit\Framework\TestCase;

final class PriceCalculatorTest extends TestCase
{
    public function testItAddsTheTaxAmount(): void
    {
        $calculator = new PriceCalculator();

        self::assertSame(1080, $calculator->total(1000, 80));
    }

    public function testItRejectsNegativeAmounts(): void
    {
        $this->expectException(InvalidArgumentException::class);

        (new PriceCalculator())->total(-1, 80);
    }
}
~~~~

### Sample 72 (sh)

~~~~sh
vendor/bin/phpunit tests
~~~~

## 15. Security, errors, and production practices

### Sample 73 (php)

~~~~php
$hash = password_hash($password, PASSWORD_DEFAULT);

if (password_verify($submittedPassword, $hash)) {
    echo "Password matched";
}
~~~~

## 16. Build a small PHP JSON service

### Sample 74 (php)

~~~~php
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
    if (preg_match('~^application/json(?:\s*;|$)~i', trim($contentType)) !== 1) {
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
~~~~

### Sample 75 (php)

~~~~php
<?php

require __DIR__ . "/index.php";
~~~~

### Sample 76 (sh)

~~~~sh
php -S 127.0.0.1:8000 -t public public/router.php
~~~~

### Sample 77 (sh)

~~~~sh
curl -i http://127.0.0.1:8000/health
~~~~

### Sample 78 (sh)

~~~~sh
curl -i -X POST http://127.0.0.1:8000/contacts -H "Content-Type: application/json" --data "{\"name\":\"Asha Rao\",\"email\":\"asha@example.test\"}"
~~~~

### Sample 79 (sh)

~~~~sh
curl -i http://127.0.0.1:8000/contacts
~~~~

