CacheFileStorage
================

[![Tests](https://github.com/atk14/CacheFileStorage/actions/workflows/tests.yml/badge.svg?branch=master)](https://github.com/atk14/CacheFileStorage/actions/workflows/tests.yml)

A PHP library for storing values in the file system, with optional expiration.

Installation
------------

    composer require atk14/cache-file-storage

Usage
-----

    $cache = new CacheFileStorage();

    $cache->write("snippet", "<b>content</b>");
    echo $cache->read("snippet"); // "<b>content</b>"

Values are serialized with `serialize()`, so any serializable value can be stored, not just strings:

    $cache->write("data", ["a" => 1, "b" => 11]);
    $cache->read("data"); // ["a" => 1, "b" => 11]

### Expiration

Pass a number of seconds as the third argument to `write()` to make the entry expire:

    $cache->write("snippet", "<b>content</b>", 60); // expires after 60 seconds

Once expired, `read()` returns `null` as if the key was never set.

### Reading without discarding the timestamp

`readInto()` returns `true`/`false` for a cache hit/miss and fills the passed-by-reference variables:

    if ($cache->readInto("snippet", $content)) {
        echo $content;
    }

The third, optional argument receives the timestamp of when the value was written:

    $cache->readInto("snippet", $content, $content_timestamp);

### Storage location

By default, entries are stored under a `cache_file_storage` subdirectory of the system temp directory — the `TEMP` constant if it's defined and non-empty, otherwise `sys_get_temp_dir()` (falling back to `/tmp`). Pass a custom directory to the constructor to override it:

    $cache = new CacheFileStorage("/path/to/cache/dir");

Licence
-------

CacheFileStorage is free software distributed [under the terms of the MIT license](http://www.opensource.org/licenses/mit-license)

[//]: # ( vim: set ts=2 et: )
