# 12. PDO, prepared statements, and transactions

[Back to notes index](../README.md)

| [Previous: Validation, sessions, cookies, and CSRF](./11-validation-sessions-cookies-and-csrf.md) | [Notes index](../README.md) | [Next: Files, uploads, and streams](./13-files-uploads-and-streams.md) |
| --- | --- | --- |

## Connect with PDO

PDO provides a common interface for PHP database drivers. For a local learning example, SQLite can store data in one file when the **pdo_sqlite** extension is enabled:

~~~php
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
~~~

Check **php -m** for the required driver. For another database, use the matching PDO driver and connection string, and keep credentials outside source control.

## Insert with a prepared statement

A prepared statement separates SQL structure from values:

~~~php
$statement = $pdo->prepare(
    "INSERT INTO contacts (name, email) VALUES (:name, :email)"
);

$statement->execute([
    "name" => "Asha Rao",
    "email" => "asha@example.test",
]);
~~~

The placeholders are filled with data separately. Never concatenate user input into SQL. Prepared statements help prevent SQL injection and make the query easier to reuse.

A placeholder represents a value. It cannot safely replace a table name or a column name. If an identifier must vary, choose it from a fixed allowlist.

## Read result rows

Use **fetch** to consume rows one at a time or **fetchAll** for a result known to be small:

~~~php
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
~~~

Do not fetch an unbounded result into memory. Add pagination or process rows incrementally when a result can grow large.

## Group changes in a transaction

A transaction makes related changes succeed or fail together:

~~~php
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
~~~

Commit after every required change succeeds. Roll back if an operation fails. The database engine and schema determine which statements can participate in a transaction, so check the driver documentation for database-specific behavior.

## Handle connection failures

Connections and queries can fail because settings are wrong, the database is unavailable, or the operation violates a constraint. Catch errors at a boundary where the application can log safe details and choose a response. Do not show connection strings, passwords, or full database errors to an untrusted web client.

## Practice

Create the local SQLite table, insert a contact with bound values, and read it by email. Try a name containing a quote and confirm the prepared statement treats it as data. Update the row inside a transaction, roll back, and check that the prior value remains.

## Check what you learned

1. What does PDO provide?
2. Which extension enables the SQLite example?
3. What does a prepared statement separate?
4. How do named placeholders receive their values?
5. Why can a placeholder not replace a table name?
6. When is fetchAll appropriate?
7. What does a transaction group?
8. Why rethrow an error after rolling back?

## References

- [PDO](https://www.php.net/manual/en/book.pdo.php)
- [PDO connections](https://www.php.net/manual/en/pdo.connections.php)
- [Prepared statements](https://www.php.net/manual/en/pdo.prepared-statements.php)
- [PDO transactions](https://www.php.net/manual/en/pdo.transactions.php)
- [PDO SQLite driver](https://www.php.net/manual/en/ref.pdo-sqlite.php)