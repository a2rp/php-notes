# 6. Arrays, strings, and JSON

[Back to notes index](../README.md)

| [Previous: Functions, scope, and closures](./05-functions-scope-and-closures.md) | [Notes index](../README.md) | [Next: Classes, objects, and properties](./07-classes-objects-and-properties.md) |
| --- | --- | --- |

## Work with indexed arrays

An indexed array stores values under integer keys:

~~~php
$prices = [12.50, 8.00, 4.25];

foreach ($prices as $price) {
    echo $price . PHP_EOL;
}

echo array_sum($prices);
~~~

Use **foreach** when processing each value. A loop variable contains the current value and does not change the original array unless the array is deliberately referenced or updated by key.

## Use associative arrays for named values

An associative array uses string keys:

~~~php
$employee = [
    "id" => 201,
    "name" => "Asha Rao",
    "active" => true,
];

echo $employee["name"];
~~~

Use the null coalescing operator when a key may be absent:

~~~php
$department = $employee["department"] ?? "Unassigned";
~~~

Do not rely on an unvalidated array from a request having every expected key or value type. Check its shape before using it.

## Transform and filter arrays

Built-in functions can express a transformation or filter:

~~~php
$numbers = [2, 5, 8, 11];

$doubled = array_map(
    fn (int $number): int => $number * 2,
    $numbers
);

$largeValues = array_filter(
    $numbers,
    fn (int $number): bool => $number >= 8
);
~~~

Read the callback and result shape carefully. **array_filter** preserves the original keys, so use **array_values** when a new sequential index is needed.

## Manipulate strings

PHP provides functions such as **trim**, **str_contains**, and **str_replace**:

~~~php
$rawName = "  Asha Rao  ";
$name = trim($rawName);

if (str_contains($name, " ")) {
    echo str_replace(" ", "-", $name);
}
~~~

The ordinary string functions often operate on bytes. For multibyte UTF-8 text, use the appropriate **mbstring** functions when character-based length or position is needed, and confirm that the extension is available.

Escaping depends on where text will be used. HTML output needs HTML escaping, while JSON output needs JSON encoding. There is no one escaping function for every context.

## Encode and decode JSON

JSON is a text format for exchanging structured data. Encode an array or object with **json_encode**:

~~~php
$payload = [
    "id" => 201,
    "name" => "Asha Rao",
];

$json = json_encode($payload, JSON_THROW_ON_ERROR);
echo $json;
~~~

Decode JSON and ask PHP to throw an exception when the input is malformed:

~~~php
$decoded = json_decode(
    '{"active":true,"visits":3}',
    true,
    512,
    JSON_THROW_ON_ERROR
);

var_dump($decoded["active"], $decoded["visits"]);
~~~

The second argument **true** asks for associative arrays. Decoding does not validate the application's expected fields. Check the resulting structure and values separately.

## Practice

Create an indexed list of scores and an associative employee record. Use **array_filter** to keep scores above a threshold. Encode the record as JSON, decode it, and test malformed JSON with **JSON_THROW_ON_ERROR**.

## Check what you learned

1. Which keys does an indexed array commonly use?
2. What kind of key names does an associative array use?
3. What happens to array_filter keys?
4. Why validate an array received from a request?
5. Why can ordinary string length functions be misleading for UTF-8 text?
6. Does one escaping function work for every output context?
7. What does JSON_THROW_ON_ERROR do?
8. Does decoding JSON validate application-specific fields?

## References

- [Arrays](https://www.php.net/manual/en/language.types.array.php)
- [Array functions](https://www.php.net/manual/en/ref.array.php)
- [Strings](https://www.php.net/manual/en/language.types.string.php)
- [Multibyte string functions](https://www.php.net/manual/en/ref.mbstring.php)
- [JSON functions](https://www.php.net/manual/en/ref.json.php)