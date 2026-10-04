# 4. Operators, conditions, and loops

[Back to notes index](../README.md)

| [Previous: Values, variables, and type declarations](./03-values-variables-and-types.md) | [Notes index](../README.md) | [Next: Functions, scope, and closures](./05-functions-scope-and-closures.md) |
| --- | --- | --- |

## Calculate and compare values

Arithmetic operators calculate numeric results. Comparison operators produce boolean results:

~~~php
$subtotal = 125.50;
$taxRate = 0.08;
$total = $subtotal + ($subtotal * $taxRate);

$isLargeOrder = $total >= 100;
echo $total;
var_dump($isLargeOrder);
~~~

Use parentheses when they make the intended order easier to read. PHP has operator precedence rules, but clear grouping is easier to review.

## Combine boolean conditions

The operators **&&** and **||** combine boolean conditions. They use short-circuit evaluation, so the right side is evaluated only when its value can affect the result:

~~~php
$age = 22;
$hasPermission = true;

if ($age >= 18 && $hasPermission) {
    echo "Access allowed";
} else {
    echo "Access denied";
}
~~~

Use parentheses when several conditions are combined. Avoid hiding assignments or complicated work inside a condition.

## Choose a branch

Use **if**, **elseif**, and **else** for conditions:

~~~php
$score = 84;

if ($score >= 90) {
    $grade = "A";
} elseif ($score >= 75) {
    $grade = "B";
} else {
    $grade = "Keep practicing";
}

echo $grade;
~~~

Use **switch** when one expression is compared against several fixed cases:

~~~php
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
~~~

A missing **break** in a switch can continue execution into the next case. Add it when fall-through is not intended.

## Repeat work with loops

A **for** loop is useful when the iteration count is known:

~~~php
for ($number = 1; $number <= 3; $number++) {
    echo "Item " . $number . PHP_EOL;
}
~~~

A **while** loop repeats while its condition stays true:

~~~php
$attempts = 0;

while ($attempts < 3) {
    echo "Attempt " . $attempts . PHP_EOL;
    $attempts++;
}
~~~

Make sure a while-loop condition can eventually become false. Otherwise, the script may run forever.

## Use break and continue

**break** leaves a loop. **continue** skips to the next iteration:

~~~php
for ($number = 1; $number <= 5; $number++) {
    if ($number === 2) {
        continue;
    }

    if ($number === 4) {
        break;
    }

    echo $number . PHP_EOL;
}
~~~

Use these sparingly. A loop with a clear condition and a small body is easier to follow.

## Practice

Write a script that classifies a numeric score using branches. Print numbers from 1 to 10, skip one value with **continue**, and stop at another with **break**. Check boundary values such as zero and the threshold itself.

## Check what you learned

1. What type of value does a comparison operator produce?
2. Why use parentheses around part of an expression?
3. How do && and || evaluate their right-hand condition?
4. Which branching structure handles several conditions?
5. What does switch compare against its case labels?
6. What can happen when break is omitted from a switch case?
7. When is a for loop useful?
8. How do break and continue differ?

## References

- [PHP operators](https://www.php.net/manual/en/language.operators.php)
- [Control structures](https://www.php.net/manual/en/language.control-structures.php)
- [Comparison operators](https://www.php.net/manual/en/language.operators.comparison.php)