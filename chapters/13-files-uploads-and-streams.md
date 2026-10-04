# 13. Files, uploads, and streams

[Back to notes index](../README.md)

| [Previous: PDO, prepared statements, and transactions](./12-pdo-prepared-statements-and-transactions.md) | [Notes index](../README.md) | [Next: Testing with PHPUnit](./14-testing-with-phpunit.md) |
| --- | --- | --- |

## Read a file from a known location

Use an explicit application path rather than a path assembled from an untrusted request:

~~~php
<?php

$path = __DIR__ . "/data/message.txt";

if (!is_file($path) || !is_readable($path)) {
    throw new RuntimeException("Practice file is unavailable");
}

$message = file_get_contents($path);
echo $message;
~~~

**file_get_contents** reads the file into memory. Use it for small, bounded data. For large files, process a stream in chunks or use an API that streams the content.

## Check an uploaded file

Uploaded files arrive in **$_FILES** with an error code and a temporary path. Check the upload result, size, and content type before storing it:

~~~php
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
~~~

The browser-provided filename and extension are untrusted. Do not use them as the destination path or proof of file type. Validate the file size at the application layer and configure PHP's upload limits as well.

## Store with a generated name outside the public directory

Save accepted files under an application-controlled name in a protected storage folder:

~~~php
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
~~~

Keep private uploads outside the web document root. If users need to download them, authorize the request and stream the file through application code. A generated storage name avoids trusting a path supplied by the client.

Content type detection is one signal, not a guarantee that a file is harmless. Apply the rules appropriate to the file type and never execute an uploaded file as server code.

## Work with a stream

A stream lets code read or write data incrementally. **fopen** returns a resource that should be closed after use:

~~~php
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
~~~

The example assumes **processChunk** handles one bounded piece of data. Streams are useful for large files because they avoid loading the whole file into one string.

## Practice

Create a small text file and read it from a known application directory. Build a form with a file input, reject an oversized file and an unsupported MIME type, and store a valid practice file under a generated name outside **public/**.

## Check what you learned

1. Why should a file path come from a known application directory?
2. When is file_get_contents appropriate?
3. What does an upload error code tell you?
4. Why check an upload's size?
5. Why can a browser filename not be trusted?
6. What does is_uploaded_file verify?
7. Where should private uploaded files be stored?
8. How can streams reduce memory use for large files?

## References

- [Filesystem functions](https://www.php.net/manual/en/ref.filesystem.php)
- [Handling file uploads](https://www.php.net/manual/en/features.file-upload.php)
- [$_FILES](https://www.php.net/manual/en/reserved.variables.files.php)
- [finfo_file](https://www.php.net/manual/en/function.finfo-file.php)
- [Streams](https://www.php.net/manual/en/book.stream.php)