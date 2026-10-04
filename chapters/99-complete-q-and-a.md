# Complete questions and answers

[Back to notes index](../README.md)

| [Previous: All code samples](./98-all-code-samples.md) | [Notes index](../README.md) | Next: End of notes |
| --- | --- | --- |

This appendix answers the review questions from every core chapter. Use the answers to check understanding, then return to the examples for context.

## 1. PHP runtime and request lifecycle

1. **Question:** What is PHP used for besides serving web pages?
   **Answer:** PHP can also run command-line scripts, scheduled jobs, and other server-side tasks.

2. **Question:** Which runtime constant reports the PHP version?
   **Answer:** PHP_VERSION is the constant that reports the current runtime version.

3. **Question:** What does PHP_SAPI report?
   **Answer:** PHP_SAPI identifies the interface through which the PHP runtime is running.

4. **Question:** Which variable contains command-line arguments?
   **Answer:** $argv contains the command-line arguments supplied to the script.

5. **Question:** Why should command-line arguments be validated?
   **Answer:** Arguments can be missing or malformed and can affect paths or commands, so validate them before use.

6. **Question:** What role can PHP-FPM play in a web request?
   **Answer:** PHP-FPM can receive PHP work forwarded by a web server and execute it in a worker process.

7. **Question:** Why should an application avoid relying on mutable worker state between requests?
   **Answer:** A worker may handle more than one request, so request-specific mutable state should not be treated as persistent storage.

8. **Question:** Why should phpinfo output not be exposed publicly?
   **Answer:** phpinfo can reveal filesystem paths, extensions, and configuration details useful to an attacker.

## 2. PHP CLI, built-in server, and configuration

1. **Question:** Which command displays the PHP runtime version?
   **Answer:** Run php --version.

2. **Question:** Which command shows the loaded CLI configuration file?
   **Answer:** Run php --ini to display the configuration file used by the CLI runtime.

3. **Question:** What does a PHP extension provide?
   **Answer:** An extension adds a set of functions or interfaces such as a database driver.

4. **Question:** What does php -l check?
   **Answer:** php -l parses a file and reports syntax errors without executing it.

5. **Question:** What does -t public select for the built-in server?
   **Answer:** -t public selects the public directory as the built-in server's document root.

6. **Question:** Why can CLI and web configurations differ?
   **Answer:** The CLI and web server can use separate php.ini files and load different extensions.

7. **Question:** Is the built-in PHP server intended for production deployment?
   **Answer:** No. It is intended for local development and testing.

8. **Question:** Where should production errors be logged?
   **Answer:** Write production diagnostics to a protected log destination.

## 3. Values, variables, and type declarations

1. **Question:** Which symbol begins a PHP variable?
   **Answer:** A variable begins with the dollar sign.

2. **Question:** Name four common PHP value types.
   **Answer:** Examples include int, float, string, bool, array, object, and null.

3. **Question:** What is type juggling?
   **Answer:** Type juggling is PHP's context-dependent conversion of a value to another type.

4. **Question:** How does === differ from ==?
   **Answer:** === checks that both type and value match; == may convert before comparing.

5. **Question:** Why should form input be converted and validated?
   **Answer:** Form values arrive as strings and may be invalid or outside the accepted range.

6. **Question:** What do parameter types communicate?
   **Answer:** They communicate the types a function expects and returns.

7. **Question:** Where should strict_types be declared?
   **Answer:** At the top of the calling file, immediately after the PHP opening tag and before other statements.

8. **Question:** What values can a nullable string accept?
   **Answer:** It accepts either a string or NULL.

## 4. Operators, conditions, and loops

1. **Question:** What type of value does a comparison operator produce?
   **Answer:** A comparison operator produces a boolean result.

2. **Question:** Why use parentheses around part of an expression?
   **Answer:** Parentheses make the intended grouping and evaluation order clear.

3. **Question:** How do && and || evaluate their right-hand condition?
   **Answer:** They evaluate the right side only when the left side does not already determine the result.

4. **Question:** Which branching structure handles several conditions?
   **Answer:** if, elseif, and else select branches based on conditions.

5. **Question:** What does switch compare against its case labels?
   **Answer:** switch compares one expression against a set of case values.

6. **Question:** What can happen when break is omitted from a switch case?
   **Answer:** Execution can continue into the next case when fall-through is not intended.

7. **Question:** When is a for loop useful?
   **Answer:** A for loop is useful when the iteration count or step is known.

8. **Question:** How do break and continue differ?
   **Answer:** break exits the loop; continue skips the rest of the current iteration and proceeds.

## 5. Functions, scope, and closures

1. **Question:** What do parameter type declarations describe?
   **Answer:** Parameter types state which value types the function accepts.

2. **Question:** What does a return type describe?
   **Answer:** A return type states the kind of value the function produces.

3. **Question:** Where should default parameter values usually appear?
   **Answer:** Optional parameters with defaults normally follow all required parameters.

4. **Question:** What is local scope?
   **Answer:** A local variable is available only within its function call or declared scope.

5. **Question:** Why pass dependencies as function parameters?
   **Answer:** Passing dependencies explicitly makes a function's inputs visible and reduces hidden global state.

6. **Question:** What does a closure capture with use?
   **Answer:** A closure captures the outer variables named in its use clause.

7. **Question:** How does an arrow function capture outer values?
   **Answer:** An arrow function automatically captures referenced outer values by value.

8. **Question:** Why can a typed parameter still need business-rule validation?
   **Answer:** A type describes shape, while a business rule may still require range or format validation.

## 6. Arrays, strings, and JSON

1. **Question:** Which keys does an indexed array commonly use?
   **Answer:** An indexed array commonly uses integer keys such as 0, 1, and 2.

2. **Question:** What kind of key names does an associative array use?
   **Answer:** An associative array uses named string keys.

3. **Question:** What happens to array_filter keys?
   **Answer:** array_filter preserves the original keys of kept values.

4. **Question:** Why validate an array received from a request?
   **Answer:** Requests are untrusted and can omit keys or send values with unexpected types.

5. **Question:** Why can ordinary string length functions be misleading for UTF-8 text?
   **Answer:** Most ordinary string operations count or address bytes, while a UTF-8 character can span multiple bytes.

6. **Question:** Does one escaping function work for every output context?
   **Answer:** No. Escaping must match the destination context such as HTML, a URL, JavaScript, or SQL.

7. **Question:** What does JSON_THROW_ON_ERROR do?
   **Answer:** It makes JSON encoding failures throw an exception that the application can handle.

8. **Question:** Does decoding JSON validate application-specific fields?
   **Answer:** No. Validate required keys, value types, and allowed ranges after decoding.

## 7. Classes, objects, and properties

1. **Question:** What does a class describe?
   **Answer:** A class describes the data and behavior that its objects share.

2. **Question:** What does the new keyword do?
   **Answer:** new creates an instance and invokes its constructor.

3. **Question:** What does $this refer to?
   **Answer:** $this refers to the current object inside an instance method.

4. **Question:** Which visibility allows any caller to use a member?
   **Answer:** Public members can be accessed by callers.

5. **Question:** Why keep a property private?
   **Answer:** Private properties let the class protect its internal state and enforce its rules.

6. **Question:** What can happen if a typed property is read before initialization?
   **Answer:** Reading an uninitialized typed property causes an error.

7. **Question:** What does a void return type communicate?
   **Answer:** The method does not return a useful value to its caller.

8. **Question:** What does composition mean in the Order example?
   **Answer:** Composition means one object contains or delegates work to another object.

## 8. Interfaces, traits, enums, and exceptions

1. **Question:** What does a class promise when it implements an interface?
   **Answer:** A class promises to provide the methods declared by the interface.

2. **Question:** Why can callers depend on an interface instead of one class?
   **Answer:** It lets callers use a stable contract without depending on one implementation class.

3. **Question:** What does a trait provide?
   **Answer:** A trait provides reusable method or property implementation to a class.

4. **Question:** How does a trait differ from an interface?
   **Answer:** An interface describes required behavior; a trait contributes implementation.

5. **Question:** What values does a backed enum represent?
   **Answer:** A backed enum gives named cases corresponding to a fixed set of scalar values.

6. **Question:** What should happen before external text is converted to an enum case?
   **Answer:** Check that the value is a permitted string before converting it to a case.

7. **Question:** When should an exception be caught?
   **Answer:** Catch it where the code can recover, translate it, or give the caller a useful response.

8. **Question:** Why should a catch block avoid hiding failures?
   **Answer:** Ignoring failures can make an unsuccessful operation appear successful and hide the cause.

## 9. Namespaces, Composer, and autoloading

1. **Question:** What problem does a namespace help prevent?
   **Answer:** A namespace groups names and reduces class-name collisions.

2. **Question:** What is the fully qualified name for App\Service\Greeting?
   **Answer:** The fully qualified class name is App\Service\Greeting.

3. **Question:** What does Composer manage?
   **Answer:** Composer resolves, installs, and autoloads PHP package dependencies.

4. **Question:** What does a PSR-4 mapping connect?
   **Answer:** It maps a namespace prefix to a directory containing matching class files.

5. **Question:** When should composer dump-autoload be run?
   **Answer:** Run it after changing Composer's autoload mapping so generated autoload files match it.

6. **Question:** What is vendor/autoload.php used for?
   **Answer:** It registers the generated class autoloader so application classes and installed packages can be loaded.

7. **Question:** What information does composer.lock record?
   **Answer:** It records the exact resolved dependency versions for repeatable installs.

8. **Question:** Why should generated files in vendor not be edited directly?
   **Answer:** Generated vendor files are replaced by Composer installs and should be regenerated from project metadata.

## 10. HTTP requests, responses, and forms

1. **Question:** Which superglobal contains the request method?
   **Answer:** $_SERVER["REQUEST_METHOD"] contains the current HTTP method.

2. **Question:** Where do GET query values appear?
   **Answer:** $_GET contains query-string values.

3. **Question:** Where do POST form values appear?
   **Answer:** $_POST contains form values sent with POST encoding.

4. **Question:** Why should $_REQUEST be avoided when the source matters?
   **Answer:** It can merge request sources according to configuration, so the origin of a value may be unclear.

5. **Question:** What does an input's name attribute control?
   **Answer:** It identifies the field key that the browser sends to the server.

6. **Question:** Why is browser-side required validation insufficient?
   **Answer:** A client can send a crafted request without using the page or browser controls.

7. **Question:** When should response headers be set?
   **Answer:** Set headers before writing response output because body output may commit the response headers.

8. **Question:** Why should untrusted text be escaped at the HTML output point?
   **Answer:** Untrusted text can contain markup, so escape it for the exact HTML context where it is rendered.

## 11. Validation, sessions, cookies, and CSRF

1. **Question:** Why is browser-side form validation insufficient?
   **Answer:** A client can bypass browser controls and send any request, so validation must run on the server.

2. **Question:** What does filter_var with FILTER_VALIDATE_EMAIL check?
   **Answer:** It checks whether a string has an email-address form according to the filter.

3. **Question:** Where is session state stored?
   **Answer:** Session data is stored on the server and associated with an identifier held by the browser.

4. **Question:** When should session_set_cookie_params be called?
   **Answer:** Call it before session_start and before sending output.

5. **Question:** Why rotate the session identifier after authentication?
   **Answer:** Rotating it after login helps prevent session fixation using an identifier chosen before authentication.

6. **Question:** What does a CSRF token help prove?
   **Answer:** It links a state-changing form submission to the current server-side session.

7. **Question:** Why use hash_equals to compare a submitted token?
   **Answer:** hash_equals performs a timing-safe string comparison.

8. **Question:** How does CSRF differ from cross-site scripting?
   **Answer:** CSRF abuses the browser's authenticated state to send an unwanted request; XSS injects executable content into a page.

## 12. PDO, prepared statements, and transactions

1. **Question:** What does PDO provide?
   **Answer:** PDO provides a common PHP interface for accessing supported database drivers.

2. **Question:** Which extension enables the SQLite example?
   **Answer:** The pdo_sqlite extension enables the SQLite connection example.

3. **Question:** What does a prepared statement separate?
   **Answer:** It keeps SQL statement structure separate from the values supplied by the application.

4. **Question:** How do named placeholders receive their values?
   **Answer:** Provide an associative array whose keys match the named placeholders.

5. **Question:** Why can a placeholder not replace a table name?
   **Answer:** Identifiers are part of SQL structure; ordinary placeholders represent data values.

6. **Question:** When is fetchAll appropriate?
   **Answer:** Use fetchAll for a result known to be small and bounded.

7. **Question:** What does a transaction group?
   **Answer:** A transaction groups related database changes so they can be committed or rolled back together.

8. **Question:** Why rethrow an error after rolling back?
   **Answer:** Rethrowing preserves the failure so a higher layer can log or translate it rather than silently hiding it.

## 13. Files, uploads, and streams

1. **Question:** Why should a file path come from a known application directory?
   **Answer:** A path from a known application directory prevents a client from selecting arbitrary filesystem locations.

2. **Question:** When is file_get_contents appropriate?
   **Answer:** It is appropriate for small files whose complete contents fit comfortably in memory.

3. **Question:** What does an upload error code tell you?
   **Answer:** UPLOAD_ERR_OK reports that PHP received the uploaded file without a reported upload error.

4. **Question:** Why check an upload's size?
   **Answer:** It prevents a client from forcing excessive memory or storage use.

5. **Question:** Why can a browser filename not be trusted?
   **Answer:** The name and extension are client-controlled and may not identify the real content or a safe path.

6. **Question:** What does is_uploaded_file verify?
   **Answer:** It checks that a file path was uploaded through PHP's HTTP upload mechanism.

7. **Question:** Where should private uploaded files be stored?
   **Answer:** Store private uploads outside the web document root.

8. **Question:** How can streams reduce memory use for large files?
   **Answer:** Streams deliver bounded chunks instead of loading the complete file at once.

## 14. Testing with PHPUnit

1. **Question:** What does PHPUnit run?
   **Answer:** PHPUnit discovers and runs tests, evaluates assertions, and reports the results.

2. **Question:** Why is PHPUnit a development dependency?
   **Answer:** It is needed for local testing but not for normal application execution.

3. **Question:** What does assertSame compare?
   **Answer:** assertSame checks both the expected value and its type against the actual value.

4. **Question:** Why use integer cents in the example?
   **Answer:** Integer cents avoid floating-point representation and formatting differences in monetary assertions.

5. **Question:** When should expectException be called?
   **Answer:** Set the expectation immediately before the call expected to throw.

6. **Question:** Why should tests not depend on execution order?
   **Answer:** Independent tests do not depend on shared state or the order in which the runner executes them.

7. **Question:** Which PHP command checks syntax without running a file?
   **Answer:** php -l parses a PHP file and checks its syntax without running it.

8. **Question:** Why does a passing test not prove every input is valid?
   **Answer:** A passing test confirms only the behavior covered by its cases and assertions.

## 15. Security, errors, and production practices

1. **Question:** What is the difference between authentication and authorization?
   **Answer:** Authentication identifies the caller; authorization decides whether that caller can perform a requested action.

2. **Question:** Why should HTML output be escaped even after validation?
   **Answer:** A value can be valid input and still contain markup that needs escaping before HTML output.

3. **Question:** Which PHP functions should store and verify a password?
   **Answer:** Use password_hash to create a stored hash and password_verify to check a submitted password.

4. **Question:** Why should production errors be logged rather than displayed to users?
   **Answer:** Detailed errors can expose paths or settings, so log them to a protected destination and return a safe response.

5. **Question:** Why should secrets stay out of committed source files?
   **Answer:** Committed secrets can be copied from repository history and used outside the intended environment.

6. **Question:** What resource limits protect a server from oversized work?
   **Answer:** Body size, upload size, query limits, and operation timeouts bound resource use.

7. **Question:** Why should uploaded files stay outside the public document root?
   **Answer:** Keeping them outside the public directory prevents direct web access and execution.

8. **Question:** What should be checked before upgrading a dependency to address an advisory?
   **Answer:** Check the advisory, affected package path, compatibility, and supported PHP constraints before changing versions.

## 16. Build a small PHP JSON service

1. **Question:** Which routes does the service expose?
   **Answer:** The service provides GET /health, GET /contacts, and POST /contacts.

2. **Question:** Why is the SQLite file outside the public directory?
   **Answer:** It prevents a browser from requesting the raw database file directly as a public asset.

3. **Question:** What does the body-size limit protect?
   **Answer:** It bounds how much request data the PHP process reads and retains.

4. **Question:** Which status code reports an oversized request?
   **Answer:** 413 Payload Too Large reports a request that exceeds the configured limit.

5. **Question:** Why is the contact insert a prepared statement?
   **Answer:** It keeps submitted values as data rather than allowing them to alter SQL statement structure.

6. **Question:** What does the Allow header tell a client?
   **Answer:** It tells a client which methods are supported for the resource.

7. **Question:** Why does the service log unexpected errors but return a generic response?
   **Answer:** The log can retain diagnostic context privately while the client receives no internal paths, SQL details, or stack trace.

8. **Question:** Which protections are still needed before a real public deployment?
   **Answer:** Add authentication, authorization, HTTPS, rate limits, migrations, monitoring, and production deployment configuration.

