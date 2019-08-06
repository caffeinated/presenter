# This package has been abandoned and is no longer maintained.

Caffeinated Presenter
=====================
[![Laravel 5.1](https://img.shields.io/badge/Laravel-5.1-orange.svg?style=flat-square)](http://laravel.com)
[![Laravel 5.2](https://img.shields.io/badge/Laravel-5.2-orange.svg?style=flat-square)](http://laravel.com)
[![Source](http://img.shields.io/badge/source-caffeinated/presenter-blue.svg?style=flat-square)](https://github.com/caffeinated/presenter)
[![License](http://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](https://tldrlegal.com/license/mit-license)

Laravel 5 view presenters, originally developed after the Laracasts video tutorial on the same topic: [View Presenters From Scratch](https://laracasts.com/lessons/view-presenters-from-scratch).

Presenters allow you to manipulate any form of data for display within a view file. A simple example would be if you have a user entity with fields for the first and last names, how would you simply display the full name of the user within your view file? The most common solution would be something like the following:

```html
<h1>Hello, {{ $user->first_name }} {{ $user->last_name }}!</h1>
```

Which works, but *every* time you need to display a user's full name, you'd have to type this out. What if instead it was something like this:

```html
<h1>Hello, {{ $user->present()->fullName }}!</h1>
```

Isn't that much more readable than the previous example? Now you may argue that you could add this type of logic directly to your model class, *which you could*, but then you'll find that your model classes are riddled with methods that are manipulating view logic. I don't believe model entities should be responsible for this. Their **only job** is to pull the requested data from the database and hand it over. *That's it.*

Quick Installation
------------------
Begin by installing the package through Composer.

```
composer require caffeinated/presenter=~2.0
```

And that's it! With your coffee in reach, start building out some awesome presenters!

Usage
-----
### 1. Pull in trait
Within your model, simply pull in the `Caffeinated\Presenter\Traits\PresentableTrait` trait, which will automatically instantiate the Caffeinated Presenter class.

```php
...

use Caffeinated\Presenter\Traits\PresentableTrait;

class Example extends Eloquent
{
	use PresentableTrait;

	...
}
```

### 2. Define your presenter class
Define a protected `$presenter` variable pointing to the namespace of your presenter class.

```php
...

use Caffeinated\Presenter\Traits\PresentableTrait;

class Example extends Eloquent
{
	use PresentableTrait;

	protected $presenter = 'App\Presenters\Page';

	...
}
```

### 3. Create your presenter class
Create a new class as defined within your model earlier - in our case we'll create a new directory within the `app` directory called `Presenters`, and create a `Page` file. Presenters should extend the abstract `Caffeinated\Presenter\Presenter` class.

```php
<?php
namespace App\Presenters;

use Caffeinated\Presenter\Presenter;

class Page extends Presenter
{
	...
}
```

### 4. Define your presenter methods
Your model instance is passed through to your presenter class automatically, and is accessible via `$this->entity`. With that, you may now define any number of presenter methods here as you wish.

```php
<?php
namespace App\Presenters;

use Caffeinated\Presenter\Presenter;

class Page extends Presenter
{
	public function title()
	{
		return ucwords($this->entity->title);
	}
}
```

The usage for the above would then be `{{ $page->present()->title }}`.
