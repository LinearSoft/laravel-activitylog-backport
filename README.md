# A backport of Spatie's Laravel-ActivityLog

This is a backport package that allows Laravel-ActivityLog to work with PHP 5.6.  By design the namespace of the original project has been left untouched `Spatie\Activitylog`.  This allows for full use of the documentation with only minor modifications:

### Installation
Change
``` bash
composer require spatie/laravel-activitylog
```
to
``` bash
composer require linearsoft/laravel-activitylog-backport
```

For your convenience the Activity facade infrastructure has been restored in this backport.
All you need to do is register the facade:

```php
// config/app.php
'aliases' => [
    ...
    'Activity' => Spatie\Activitylog\ActivitylogFacade::class,
];
```

### Testing

All testing has been stripped from the backport version.

### Bugs or features requests

For the most part most all bugs & requests should be submitted to the [Spatie Team](https://github.com/spatie/laravel-activitylog/issues). And the fixes/features will eventually be backported into this project.

If, however, you found a problem specifically with the backport version please it via [GitHub](https://github.com/LinearSoft/entrust-cli/issues)

### Licensing

The licensing has been modified from MIT to the GPLv3 License - see the `LICENSE` file for details.
You are requested, however, to still follow the Postcardware "requirements" of the original package.

# START OF ORIGINAL ReadMe.md

#### Log activity inside your Laravel app

The `spatie/laravel-activity` package provides easy to use functions to log the activities of the users of your app. It can also automatically log model events. All activity will be stored in the `activity_log` table.

Here's a litte demo of how you can use it:

```php
activity()->log('Look, I logged something');
```

You can retrieve all activity using the `Spatie\Activitylog\Models\Activity` model.

```php
Activity::all();
```

Here's a more advanced example:
```php
activity()
   ->performedOn($anEloquentModel)
   ->causedBy($user)
   ->withProperties(['customProperty' => 'customValue'])
   ->log('Look, I logged something');
   
$lastLoggedActivity = Activity::all()->last();

$lastLoggedActivity->subject; //returns an instance of an eloquent model
$lastLoggedActivity->causer; //returns an instance of your user model
$lastLoggedActivity->getExtraProperty('customProperty'); //returns 'customValue'
$lastLoggedActivity->description; //returns 'Look, I logged something'
```


Here's an example on [event logging](https://docs.spatie.be/laravel-activitylog/v1/advanced-usage/logging-model-events).

```php
$newsItem->name = 'updated name';
$newsItem->save();

//updating the newsItem will cause an activity being logged
$activity = Activity::all()->last();

$activity->description; //returns 'updated'
$activity->subject; //returns the instance of NewsItem that was created
```

Calling `$activity->changes` will return this array:

```php
[
   'attributes' => [
        'name' => 'updated name',
        'text' => 'Lorum',
    ],
    'old' => [
        'name' => 'original name',
        'text' => 'Lorum',
    ],
];
```

Spatie is a webdesign agency based in Antwerp, Belgium. You'll find an overview of all our open source projects [on our website](https://spatie.be/opensource).

## Postcardware

You're free to use this package ~~(it's MIT-licensed)~~You're free to use this package (it's [MIT-licensed](LICENSE.md)), but if it makes it to your production environment we highly appreciated you sending us a postcard from your hometown, mentioning which of our package(s) you are using.

Our address is: Spatie, Samberstraat 69D, 2060 Antwerp, Belgium.

The best postcards will get published on the open source page on our website.

## Documentation
You'll find the documentation on [https://docs.spatie.be/laravel-activitylog/v1](https://docs.spatie.be/laravel-activitylog/v1).

Find yourself stuck using the package? Found a bug? Do you have general questions or suggestions for improving the activity log? Feel free to [create an issue on GitHub](https://github.com/spatie/laravel-activitylog/issues), we'll try to address it as soon as possible.

If you've found a bug regarding security please mail [freek@spatie.be](mailto:freek@spatie.be) instead of using the issue tracker.


## Installation

You can install the package via composer:

``` bash
composer require spatie/laravel-activitylog
```

Next, you must install the service provider:

```php
// config/app.php
'providers' => [
    ...
    Spatie\Activitylog\ActivitylogServiceProvider::class,
];
```

You can publish the migration with:
```bash
php artisan vendor:publish --provider="Spatie\Activitylog\ActivitylogServiceProvider" --tag="migrations"
```

*Note*: The default migration assumes you are using integers for your model IDs. If you are using UUIDs, or some other format, adjust the format of the subject_id and causer_id fields in the published migration before continuing.

After the migration has been published you can create the `activity_log` table by running the migrations:


```bash
php artisan migrate
```

You can optionally publish the config file with:
```bash
php artisan vendor:publish --provider="Spatie\Activitylog\ActivitylogServiceProvider" --tag="config"
```

This is the contents of the published config file:

```php
return [

    /**
     * When set to false, no activities will be saved to database.
     */
    'enabled' => env('ACTIVITY_LOGGER_ENABLED', true),

    /**
     * When running the clean-command all recording activites older than
     * the number of days specified here will be deleted.
     */
    'delete_records_older_than_days' => 365,


    /**
     * When not specifying a log name when logging activity
     * we'll using this log name.
     */
    'default_log_name' => 'default',


    /**
     * When set to true, the subject returns soft deleted models.
     */
     'subject_returns_soft_deleted_models' => false,
     
     
    /**
     * This model will be used to log activity. The only requirement is that
     * it should be or extend the Spatie\Activitylog\Models\Activity model.
     */
    'activity_model' => \Spatie\Activitylog\Models\Activity::class,     
];
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email freek@spatie.be instead of using the issue tracker.

## Credits

- [Freek Van der Herten](https://github.com/freekmurze)
- [Sebastian De Deyne](https://github.com/sebastiandedeyne)
- [All Contributors](../../contributors)

## About Spatie
Spatie is a webdesign agency based in Antwerp, Belgium. You'll find an overview of all our open source projects [on our website](https://spatie.be/opensource).

## License

~~The MIT License (MIT). Please see License File for more information~~.
