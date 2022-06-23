# Introduction

[`Phan`](https://github.com/phan/phan) is a static analyzer for PHP that prefers to minimize false-positives. `Phan` attempts to prove incorrectness rather than correctness.

`Phan` looks for common issues and will verify type compatibility on various operations when type information is available or can be deduced. `Phan` has a good (but not comprehensive) understanding of flow control and can track values in a few use cases (e.g. arrays, integers, and strings).

# Prerequisites

- [PHP 8.1](https://www.php.net/releases/8.1/en.php)
- [Composer](https://getcomposer.org/)
- [Laravel 8](https://laravel.com/docs/8.x/releases)

Laravel 8 is used for the development of web applications in PHP.

> Lumen a micro-framework can also be used depending on project need. This document is only verified with Laravel 8.

# Installation

Install the following package:

- [phan/phan](https://github.com/phan/phan)

**`phan/phan`**

```sh
composer require phan/phan --dev
```

Make sure to [install dependencies](https://github.com/phan/phan/wiki/Getting-Started#installing-phan-dependencies) for `Phan`.

# Configuration

Create `.phan/config.php` file in the root directory of your project.

Then add the below configuration in this file.

```php
<?php

/**
 * This configuration will be read and overlaid on top of the
 * default configuration. Command line arguments will be applied
 * after this file is read.
 */
return [

    // Supported values: `'5.6'`, `'7.0'`, `'7.1'`, `'7.2'`, `'7.3'`, `'7.4'`,
    // `'8.0'`, `'8.1'`, `null`.
    // If this is set to `null`,
    // then Phan assumes the PHP version which is closest to the minor version
    // of the php executable used to execute Phan.
    "target_php_version" => '8.1',

    // A list of directories that should be parsed for class and
    // method information. After excluding the directories
    // defined in exclude_analysis_directory_list, the remaining
    // files will be statically analyzed for errors.
    //
    // Thus, both first-party and third-party code being used by
    // your application should be included in this list.
    
    // Note - Keep adding to the vendor section below as we add more dependencies
    // based on the errors you encounter when you run phan. 
    
    'directory_list' => [
        'app',
        'config',
        'database',
        'public',
        'resources',
        'routes',
        'storage',
        'tests',
        'bootstrap',
        'vendor/laravel',
        'vendor/fakerphp'
        'vendor/fruitcake',
        'vendor/monolog',
        'vendor/nesbot',
        'vendor/ramsey',
        'vendor/psr'
        'vendor/symfony',        
    ],

    // A directory list that defines files that will be excluded
    // from static analysis, but whose class and method
    // information should be included.
    //
    // Generally, you'll want to include the directories for
    // third-party code (such as "vendor/") in this list.
    //
    // n.b.: If you'd like to parse but not analyze 3rd
    //       party code, directories containing that code
    //       should be added to the `directory_list` as
    //       to `exclude_analysis_directory_list`.
    "exclude_analysis_directory_list" => [
        'vendor'
    ],

    // A list of plugin files to execute.
    // Plugins which are bundled with Phan can be added here by providing their name
    // (e.g. 'AlwaysReturnPlugin')
    //
    // Documentation about available bundled plugins can be found
    // at https://github.com/phan/phan/tree/v5/.phan/plugins
    //
    // Alternately, you can pass in the full path to a PHP file
    // with the plugin's implementation.
    // (e.g. 'vendor/phan/phan/.phan/plugins/AlwaysReturnPlugin.php')
    'plugins' => [
        // checks if a function, closure or method unconditionally returns.
        // can also be written as 'vendor/phan/phan/.phan/plugins/AlwaysReturnPlugin.php'
        'AlwaysReturnPlugin',
        'DollarDollarPlugin',
        'DuplicateArrayKeyPlugin',
        'DuplicateExpressionPlugin',
        'PregRegexCheckerPlugin',
        'PrintfCheckerPlugin',
        'SleepCheckerPlugin',
        // Checks for syntactically unreachable statements in
        // the global scope or function bodies.
        'UnreachableCodePlugin',
        'UseReturnValuePlugin',
        'EmptyStatementListPlugin',
        'LoopVariableReusePlugin',
    ],
    'suppress_issue_types' => [
		// The following two have been added to not highlight issues
		// raised due to variables added for interface methods that are
		// then not used by the interface implementation
		'PhanUnusedPublicMethodParameter',
		'PhanUnusedProtectedMethodParameter',
		'PhanUnusedPrivateMethodParameter',
		// Allow unused values in foreach
		'PhanUnusedVariableValueOfForeachWithKey'		
	],
	// Backwards Compatibility Checking
	// (Disable this if the application no longer supports php 5,
	// or use a different tool.
	// Phan's checks are currently slow)
	// Set it to false or omit it.
	'backward_compatibility_checks' => false,
	'unused_variable_detection' => true
];

```

# Usage

Add script command to run `phan` in `composer.json` file.

```json
"scripts": {
    // ..<existing scripts>
    "phan": "phan",
  },
```

**`composer run phan`**

This command will analyze and display all the issues if any.
