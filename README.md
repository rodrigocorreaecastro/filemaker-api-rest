# filemaker-api-rest
FileMaker API Rest PHP


## Installation

### Using Composer
You can use the `composer` package manager to install. Either run:

    $ php composer.phar require airmoi/filemaker "*"

or add:

    "airmoi/filemaker": "^2.2"

to your composer.json file

### Manual Install

You can also manually install the API easily to your project. Just download the source [ZIP](https://github.com/airmoi/FileMaker/archive/master.zip) and extract its content into your project.

## Usage

STEP 1 : Read the 'Important Notice' below

STEP 2 : include the API autoload

```php
require '/path/to/autoloader.php';
```
*This step is facultative if you are using composer*

STEP 3 : Create a FileMaker instance
```php
use airmoi\FileMaker\FileMaker;

$fm = new FileMaker($database, $host, $username, $password, $options);
```

STEP 4 : use it quite the same way you would use the offical API...

...And enjoy code completion using your favorite IDE and php 7 support without notice/warnings.

You may also find sample usage by reading the `sample.php` file located in the "demo" folder 

### Sample demo code

```php
use airmoi\FileMaker\FileMaker;
use airmoi\FileMaker\FileMakerException;

require('/path/to/autoloader.php');

$fm = new FileMaker('database', 'localhost', 'filemaker', 'filemaker', ['prevalidate' => true]);

try {
    $command = $fm->newFindCommand('layout_name');
    $records = $command->execute()->getRecords(); 
    
    foreach($records as $record) {
        echo $record->getField('fieldname');
        ...
    }
} 
catch (FileMakerException $e) {
    echo 'An error occured ' . $e->getMessage() . ' - Code : ' . $e->getCode();
}
```
