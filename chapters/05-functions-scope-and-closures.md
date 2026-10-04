# 5. Functions, scope, and closures

[Back to notes index](../README.md)

| [Previous: Operators, conditions, and loops](./04-operators-conditions-and-loops.md) | [Notes index](../README.md) | [Next: Arrays, strings, and JSON](./06-arrays-strings-and-json.md) |
| --- | --- | --- |

## Define a reusable function

A function names a piece of behavior that can be called from different parts of a program:

~~~php
<?php

declare(strict_types=1);

function totalWithTax(float $amount, float $rate): float
{
    return $amount + ($amount * $rate);
}

echo totalWithTax(100.0, 0.08);
~~~

The parameter declarations describe the values the function expects. The return type describes the value it produces. Keep a function focused on one understandable job.

## Use default and nullable parameters

A default value makes a trailing parameter optional:

~~~php
function greet(string $name = "friend"): string
{
    return "Hello, " . $name;
}

echo greet();
echo greet("Asha");
~~~

Use a nullable type when NULL is a valid input:

~~~php
function displayLabel(?string $label): string
{
    return $label ?? "Untitled";
}
~~~

Choose a default that has a clear meaning. Do not use a placeholder default to hide missing required data.

## Understand local scope

Variables created inside a function are local to that call:

~~~php
function makeMessage(string $name): string
{
    $greeting = "Hello";
    return $greeting . ", " . $name;
}

echo makeMessage("Ravi");
~~~

Pass values through parameters and return results explicitly. This makes dependencies visible and avoids functions that depend on mutable global state.

## Use a closure when behavior is a value

An anonymous function can be assigned to a variable and called later. **use** captures an outer variable:

~~~php
$taxRate = 0.08;

$addTax = function (float $amount) use ($taxRate): float {
    return $amount + ($amount * $taxRate);
};

echo $addTax(100.0);
~~~

The closure captures **$taxRate** by value in this example. To share a mutable variable, capture it by reference with **use (&$value)** and do so only when that shared mutation is needed.

An arrow function is a shorter closure form. It captures outer variables by value automatically:

~~~php
$factor = 3;
$multiply = fn (int $value): int => $value * $factor;

echo $multiply(4);
~~~

Use the longer closure form when the body needs multiple statements or explicit capture behavior.

## Validate function inputs

A type declaration checks a type shape, not every business rule. Validate values such as ranges and non-empty strings:

~~~php
function percentageOf(float $amount, float $percent): float
{
    if ($amount < 0 || $percent < 0 || $percent > 100) {
        throw new InvalidArgumentException("Values are outside the allowed range");
    }

    return $amount * ($percent / 100);
}
~~~

Throw a clear exception when the caller supplies an invalid value. The caller can catch it where it has enough context to respond.

## Practice

Write a function that accepts a product price and discount percentage, validates both, and returns the final price. Make the discount percentage optional only if zero is a meaningful default. Add a closure that applies a chosen tax rate.

## Check what you learned

1. What do parameter type declarations describe?
2. What does a return type describe?
3. Where should default parameter values usually appear?
4. What is local scope?
5. Why pass dependencies as function parameters?
6. What does a closure capture with use?
7. How does an arrow function capture outer values?
8. Why can a typed parameter still need business-rule validation?

## References

- [User-defined functions](https://www.php.net/manual/en/functions.user-defined.php)
- [Function arguments](https://www.php.net/manual/en/functions.arguments.php)
- [Anonymous functions](https://www.php.net/manual/en/functions.anonymous.php)
- [Arrow functions](https://www.php.net/manual/en/functions.arrow.php)
- [Exceptions](https://www.php.net/manual/en/language.exceptions.php)