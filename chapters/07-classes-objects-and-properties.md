# 7. Classes, objects, and properties

[Back to notes index](../README.md)

| [Previous: Arrays, strings, and JSON](./06-arrays-strings-and-json.md) | [Notes index](../README.md) | [Next: Interfaces, traits, enums, and exceptions](./08-interfaces-traits-enums-and-exceptions.md) |
| --- | --- | --- |

## Define a class and create an object

A class describes the data and behavior shared by its objects. An object is one instance of that class:

~~~php
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
~~~

The **new** keyword creates an object and runs its constructor. **$this** refers to the current object inside an instance method.

## Encapsulate state with visibility

A **public** member can be used by callers. A **private** member can be used only inside its class. A **protected** member is available inside the class and its subclasses.

The **cents** property is private, so callers use **cents()** or **format()** instead of changing the internal value directly. This lets the class enforce its own rules.

Typed properties declare what kind of value a property stores. Initialize a typed property before reading it. An uninitialized typed property causes an error.

## Use methods to express behavior

Instance methods operate on an object. A method can validate input, update state, or return a value:

~~~php
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
~~~

A **void** return type means the method does not return a useful value to its caller.

## Prefer composition for collaborating behavior

A class can hold another object and delegate work to it. This is composition:

~~~php
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
~~~

An order contains a Money value rather than duplicating its formatting and validation logic. Give each class a clear responsibility and keep object relationships straightforward.

## Practice

Create a **Money** object for a valid number of cents. Try a negative value and confirm that construction fails. Add an order object that stores a Money value, then read the formatted total without changing the private property.

## Check what you learned

1. What does a class describe?
2. What does the new keyword do?
3. What does $this refer to?
4. Which visibility allows any caller to use a member?
5. Why keep a property private?
6. What can happen if a typed property is read before initialization?
7. What does a void return type communicate?
8. What does composition mean in the Order example?

## References

- [Classes and objects](https://www.php.net/manual/en/language.oop5.php)
- [Properties](https://www.php.net/manual/en/language.oop5.properties.php)
- [Constructors and destructors](https://www.php.net/manual/en/language.oop5.decon.php)
- [Visibility](https://www.php.net/manual/en/language.oop5.visibility.php)