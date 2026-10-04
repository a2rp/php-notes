# 8. Interfaces, traits, enums, and exceptions

[Back to notes index](../README.md)

| [Previous: Classes, objects, and properties](./07-classes-objects-and-properties.md) | [Notes index](../README.md) | [Next: Namespaces, Composer, and autoloading](./09-namespaces-composer-and-autoloading.md) |
| --- | --- | --- |

## Describe a contract with an interface

An interface names methods that an implementing class promises to provide:

~~~php
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
~~~

A caller can depend on the **Notifier** contract instead of a particular implementation. This makes it easier to replace a component or provide a test double.

## Share implementation with a trait

A trait provides methods that can be included in more than one class:

~~~php
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
~~~

Traits reuse implementation, but they do not define an object contract. Prefer an interface when callers need to rely on behavior. Keep traits small because a class can become hard to understand when behavior comes from many traits.

## Represent fixed choices with an enum

A backed enum gives a finite set of named cases with scalar values. Enums require a PHP release that supports them:

~~~php
enum OrderStatus: string
{
    case Draft = "draft";
    case Paid = "paid";
    case Shipped = "shipped";
}

$status = OrderStatus::Paid;
echo $status->value;
~~~

Use enum cases instead of loosely related strings when the application has a well-defined set of states. Validate external strings before converting them to an enum case.

## Raise and handle exceptions

An exception represents a failure that a caller can handle:

~~~php
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
~~~

Catch a specific exception when the code can recover or respond. Do not catch every failure and return a success-shaped value. If an error cannot be handled at that layer, let it reach a layer that can make the right decision.

A custom exception can add domain meaning:

~~~php
final class MissingOrder extends RuntimeException
{
}
~~~

Throw **MissingOrder** when a requested order does not exist, then translate it into the appropriate CLI or HTTP response at the application boundary.

## Practice

Create a payment method interface with two implementations. Add an enum for order state. Try to turn an invalid external string into a status and handle the failure. Add one custom exception for a missing record.

## Check what you learned

1. What does a class promise when it implements an interface?
2. Why can callers depend on an interface instead of one class?
3. What does a trait provide?
4. How does a trait differ from an interface?
5. What values does a backed enum represent?
6. What should happen before external text is converted to an enum case?
7. When should an exception be caught?
8. Why should a catch block avoid hiding failures?

## References

- [Object interfaces](https://www.php.net/manual/en/language.oop5.interfaces.php)
- [Traits](https://www.php.net/manual/en/language.oop5.traits.php)
- [Enumerations](https://www.php.net/manual/en/language.enumerations.php)
- [Exceptions](https://www.php.net/manual/en/language.exceptions.php)