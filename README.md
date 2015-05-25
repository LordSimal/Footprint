# Footprint

[![Build Status](https://img.shields.io/travis/UseMuffin/Footprint/master.svg?style=flat-square)](https://travis-ci.org/UseMuffin/Footprint)
[![Coverage](https://img.shields.io/coveralls/UseMuffin/Footprint/master.svg?style=flat-square)](https://coveralls.io/r/UseMuffin/Footprint)
[![Total Downloads](https://img.shields.io/packagist/dt/muffin/footprint.svg?style=flat-square)](https://packagist.org/packages/muffin/footprint)
[![License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)

This plugin allows you to pass the currently logged in user to the model layer of a CakePHP 3
application.

It comes bundled with the `FootprintBehavior` to allow you control over columns such as `user_id`, 
`created_by`, `company_id` just like the core's `TimestampBehavior`.

## Install

Using [Composer][composer]:

```
composer require muffin/footprint:dev-master
```

You then need to load the plugin. In `boostrap.php`, something like:

```php
\Cake\Core\Plugin::load('Muffin/Footprint');
```

## Usage

### Trait

First, you will need to include the `Muffin\Footprint\Auth\FootprintAwareTrait` to your `AppController`:

```php
use Muffin\Footprint\Auth\FootprintAwareTrait;

class AppController extends Controller
{
	use FootprintAwareTrait;
}
```

This will attach the `Muffin\Footprint\Event\FootprintListener` to models
which will inject the currently logged in user's instance on `Model.beforeSave` and `Model.afterSave` in the `_footprint`
key of `$options`. 

### Behavior

To use the included behavior to automatically update the `created_by` and `updated_by` fields of a record for example,
add the following to your table's `initialize()` method:

```php
$this->addBehavior('Muffin/Footprint.Footprint');
```

You can customize that like so:

```php
$this->addBehavior('Footprint', [
    'events' => [
        'Model.beforeSave' => [
        	'user_id' => 'new',
            'company_id' => 'new',
            'modified_by' => 'always'
        ]
    ],
    'propertiesMap' => [
        'company_id' => '_footprint.company.id',
    ],
]);
```

This will insert the currently logged in user's primary key in `user_id` and `modified_by` fields when creating 
a record, on the `modified_by` field again when updating the record and it will use the associated user record's
company `id` in the `company_id` field when creating a record.

## Patches & Features

* Fork
* Mod, fix
* Test - this is important, so it's not unintentionally broken
* Commit - do not mess with license, todo, version, etc. (if you do change any, bump them into commits of
their own that I can ignore when I pull)
* Pull request - bonus point for topic branches

## Bugs & Feedback

http://github.com/usemuffin/footprint/issues

## Credits

This was initially inspired by [Ceeram/Blame] which was just a proof of concept and in such, was not easy
(or even possible) to extend/use for more than the `modified_by` and `created_by` fields.

## License

Copyright (c) 2015, [Use Muffin][muffin] and licensed under [The MIT License][mit].

[cakephp]:http://cakephp.org
[composer]:http://getcomposer.org
[mit]:http://www.opensource.org/licenses/mit-license.php
[muffin]:http://usemuffin.com
[Ceeram/Blame]:http://github.com/ceeram/blame