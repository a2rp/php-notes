# 3. Values, variables, and type declarations

[Back to notes index](../README.md)

| [Previous: PHP CLI, built-in server, and configuration](./02-php-cli-server-and-configuration.md) | [Notes index](../README.md) | [Next: Operators, conditions, and loops](./04-operators-conditions-and-loops.md) |
| --- | --- | --- |

## Assign values to variables

PHP variables begin with a dollar sign. Their types come from the values assigned to them:

~~~php
<?php

$name = "Ashish";
$years = 27;
$isLearning = true;
$score = null;

var_dump($name, $years, $isLearning, $score);
~~~

Common built-in types include **int**, **float**, **string**, **bool**, **array**, **object**, and **null**. PHP can convert values in some contexts through type juggling. The conversion rules depend on the operation, so inspect and validate values that come from users or external systems.

Use **get_debug_type()** while learning to see a value's runtime type:

~~~php
$value = "42";
echo get_debug_type($value);
~~~

## Compare strictly when type matters

Loose comparison **==** may convert values before comparing them. Strict comparison **===** checks both the value and the type:

~~~php
$input = "0";

var_dump($input == 0);
var_dump($input === 0);
~~~

A form value is normally a string, even if the user typed digits. Convert and validate it before treating it as an integer. Strict comparisons help avoid surprising branches.

## Declare parameter and return types

Parameter and return declarations make a function's expectations visible:

~~~php
<?php

declare(strict_types=1);

function add(int $left, int $right): int
{
    return $left + $right;
}

echo add(2, 3);
~~~

In strict mode, scalar argument checking follows the strictness of the calling file. Put **declare(strict_types=1)** at the top of each application and test file where strict calls are expected. Type declarations improve clarity, but they do not replace validation of untrusted input.

## Use nullable and union types deliberately

A nullable type can accept either the declared type or NULL. A union type allows one of several stated types:

~~~php
function displayName(?string $name): string
{
    return $name ?? "Guest";
}

function formatIdentifier(int|string $identifier): string
{
    return (string) $identifier;
}
~~~

Use the narrowest type that represents the real input. A type declaration should describe valid program values, not silently accept every possible value.

## Inspect arrays and values

Arrays can be indexed by integers or strings. A later chapter covers their structure in detail. For now, inspect a small example:

~~~php
$person = [
    "name" => "Asha",
    "active" => true,
    "visits" => 3,
];

var_dump($person);
~~~

Use readable variable names and assign values with one clear meaning. Avoid reusing a variable for unrelated types or purposes.

## Practice

Create variables for a name, an integer, a decimal amount, and a boolean. Print their types. Compare a numeric string and an integer with both **==** and **===**. Add a typed function and try calling it with a value that does not meet its declared type.

## Check what you learned

1. Which symbol begins a PHP variable?
2. Name four common PHP value types.
3. What is type juggling?
4. How does === differ from ==?
5. Why should form input be converted and validated?
6. What do parameter types communicate?
7. Where should strict_types be declared?
8. What values can a nullable string accept?

## References

- [PHP type system](https://www.php.net/manual/en/language.types.php)
- [Type declarations](https://www.php.net/manual/en/language.types.declarations.php)
- [Strict typing](https://www.php.net/manual/en/language.types.declarations.php#language.types.declarations.strict)
- [Comparison operators](https://www.php.net/manual/en/language.operators.comparison.php)