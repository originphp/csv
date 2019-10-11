# CSV

![license](https://img.shields.io/badge/license-MIT-brightGreen.svg)
[![build](https://travis-ci.org/originphp/csv.svg?branch=masterhttps://travis-ci.org/originphp/csv.svg?branch=master)](https://travis-ci.org/originphp/csv)
[![coverage](https://coveralls.io/repos/github/originphp/csv/badge.svg?branch=master)](https://coveralls.io/github/originphp/csv?branch=master)

The CSV utility makes it easy to work with CSV files.

## Installation

To install this package

```linux
$ composer require origin/csv
```

## Create CSV from an Array

To use an array to create CSV data

```php
use Origin\Csv\Csv;

$csv = Csv::fromArray([
    ['jim','jim@example.com'],
    ['tony','tony@example.com']
]);

```

Which will give you this

```
jim,jim@example.com
tony,tony@example.com
```

You can also use keys from the array as headers

```php
use Origin\Csv\Csv;

$csv = Csv::fromArray([
        ['name'=>'jim','email'=>'jim@example.com'],
        ['name'=>'tony','email'=>'tony@example.com']
    ],['header'=>true]);

```

Which will return this

```
name,email
jim,jim@example.com
tony,tony@example.com
```

If you want to use custom headers

```php
use Origin\Csv\Csv;

$csv = Csv::fromArray([
        ['name'=>'jim','email'=>'jim@example.com'],
        ['name'=>'tony','email'=>'tony@example.com']
    ],['header'=>['First Name','Email Address']]);

```

Which will gives you this

```
"First Name","Email Address"
jim,jim@example.com
tony,tony@example.com
```

## Create an Array from CSV Data

Use the `toArray` method to create an array using CSV data.

```php
use Origin\Csv\Csv;

$csv = file_get_contents('/path/file.csv');
$data = Csv::toArray($csv);

```

If the CSV file has a header row, then you can skip it by passing an options array with the key header set to true.

```php
$data = Csv::toArray($csv,['header'=>true]);
```

If you want to use the headers as keys for each record in the array, this will use the first row as the keys for the array.

```php
$data = Csv::toArray($csv,['header'=>true,'keys'=>true]);
```

If you want to set custom keys for each record in the array

```php
$data = Csv::toArray($csv,['keys'=>['First Name','Email Address']]);
```

## Processing Large Files

To process large CSV files in a memory efficient way use the `process` method, which takes the the same options as `toArray`. The difference here is that, it will reads the CSV file one line at a time, returns its for processing, then goes the next.

```php
$rows = Csv::process('/path/to/file.csv',['keys'=>['First Name','Email Address']]);
foreach($rows as $row){
    ... do something
}
```