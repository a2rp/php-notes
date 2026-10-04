# 14. Testing with PHPUnit

[Back to notes index](../README.md)

| [Previous: Files, uploads, and streams](./13-files-uploads-and-streams.md) | [Notes index](../README.md) | [Next: Security, errors, and production practices](./15-security-errors-and-production.md) |
| --- | --- | --- |

## Install a test runner for the project

PHPUnit is a test framework that runs PHP tests and reports whether assertions pass. Add it as a development dependency with Composer:

~~~sh
composer require --dev phpunit/phpunit
~~~

The compatible PHPUnit release depends on the PHP runtime. Check the PHPUnit version requirements before installing it. The application can install its development tools from **composer.lock** without placing them in production.

## Create a small class to test

Create **src/Billing/PriceCalculator.php**:

~~~php
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
~~~

The example uses integer cents so the expected values do not depend on floating-point formatting.

## Write behavior-focused tests

Create **tests/PriceCalculatorTest.php**:

~~~php
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
~~~

A test should describe behavior, arrange the needed values, run the action, and assert the observable result. The exception expectation is set immediately before the call expected to throw.

## Run the tests

Run PHPUnit from the project root:

~~~sh
vendor/bin/phpunit tests
~~~

On Windows, Composer also creates a **vendor/bin/phpunit.bat** command. Use the command path generated for the current operating system.

A failed assertion should identify what result differed. A passing test only checks the cases that were written; it does not prove every input is correct.

## Keep tests isolated

Tests should not rely on another test running first. Create the data each test needs and clean up resources such as temporary files, database connections, and local servers.

For database tests, use a disposable test database or isolated transaction strategy. Do not point destructive test setup at a production database.

## Debug a failing test

Use the failure message and stack trace to find the failing expectation. **var_dump** can help during local investigation, but remove temporary dumps before committing. Log useful details without exposing passwords, tokens, or private user data.

Use **php -l** to check syntax, then run the test suite. Linting and tests answer different questions.

## Practice

Install PHPUnit as a development dependency, create the calculator and test files, then run **vendor/bin/phpunit tests**. Change an expected total temporarily and read the assertion output. Restore the correct expected value and add a boundary case.

## Check what you learned

1. What does PHPUnit run?
2. Why is PHPUnit a development dependency?
3. What does assertSame compare?
4. Why use integer cents in the example?
5. When should expectException be called?
6. Why should tests not depend on execution order?
7. Which PHP command checks syntax without running a file?
8. Why does a passing test not prove every input is valid?

## References

- [PHPUnit manual](https://docs.phpunit.de/en/)
- [Writing tests](https://docs.phpunit.de/en/latest/writing-tests-for-phpunit.html)
- [Composer dependencies](https://getcomposer.org/doc/01-basic-usage.md#installing-dependencies)
- [PHP command-line options](https://www.php.net/manual/en/features.commandline.options.php)