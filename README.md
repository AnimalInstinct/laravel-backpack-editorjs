# Laravel Backpack EditorJs field

Package adds new editorjs field to the Laravel Backpack admin panel.

https://github.com/animalinstinct/laravel-backpack-editorjs

## Requirements

- Laravel-Editor.js git@github.com:alaminfirdows/laravel-editorjs.git

Laravel-Editor.js uses to render editor's output.

## Installation

Install and configure parser first, More information [https://github.com/alaminfirdows/laravel-editorjs](on the package page)

```bash
composer require alaminfirdows/laravel-editorjs
php artisan vendor:publish --tag="laravel_editorjs-config"
```
Install this package

```bash
composer require animalinstinct/laravel-backpack-editorjs
```

### For laravel 9,10

Add the LaravelBackpackEditorJsServiceProvider to the service providers list.

```php
'providers' => [
    // ...
    AnimalInstinct\LaravelBackpackEditorJs\LaravelBackpackEditorJsServiceProvider::class,
    // ...
]
```

Run the install script

```bash
php artisan backpack-editorjs:install
```

## Configuration

Add middleware to the Backpack base config. The middleware would set context to not use the Laravel-Editor.js parser for the backpack routes.

```php
    'middleware_class' => [
        //...
        \AnimalInstinct\LaravelBackpackEditorJs\app\Http\Middlewares\SetEditorJsParserContext::class,
        //...
    ]
```

## Usage

In the Backpack controllers, use 'editorjs' as a field type

```php
//YourAnyCrudController.php

$this->crud->addField([
    'name' => 'description',
    'label' => 'Description',
    'type' => 'editorjs',
]);
```
In the model import the trait, use it, and declare the fields would be used as editorjs fields for the parser could do it's job.

```php
//YourAnyModel.php

// Import the trait
use AnimalInstinct\LaravelBackpackEditorJs\app\Models\Traits\HasEditorJsFields;

// Use it in the model
use HasEditorJsFields;

// Declare the fields
protected $editorJsFields = ['description','anyfield'];
```

### Backup fields

The backup field will be used if the field value is not a valid JSON (for cases when you have another editor before and desire to save it as is). To set that kind a field import EditorJsField trait, use it in your CrudController, the call the trait method EditorJsField::withBackup() in the field type.

The first argument is a current model, then field name and backup field to show instead.

```php
//YourAnyCrudController.php
use AnimalInstinct\LaravelBackpackEditorJs\app\Http\Controllers\Traits\EditorJsField;

use EditorJsField;

$this->crud->addField([
    'name' => 'description',
    'label' => 'Description',
    'type' => EditorJsField::withBackup($this->crud->getCurrentEntry(), 'description', 'ckeditor'),
]);