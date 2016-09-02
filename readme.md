# Larasearch

## Introduction

This package is forked from [Laravel Scout](https://github.com/laravel/scout).

Larasearch based on the official Laravel Scout which provides a simple, driver based solution for adding full-text search to your Eloquent models to supports Laravel 5.2 and [Elasticsearch](https://www.elastic.co/) Engine.

## Installation

First, install the Larasearch via the Composer package manager:

    composer require gtk/larasearch

Next, you should add the `LarasearchServiceProvider` to the `providers` array of your `config/app.php` configuration file:

    Gtk\Larasearch\LarasearchServiceProvider::class,

After registering the Larasearch service provider, you should publish the Larasearch configuration using the `vendor:publish` Artisan command. This command will publish the `larasearch.php` configuration file to your `config` directory:

    php artisan vendor:publish

Finally, add the `Gtk\Larasearch\Searchable` trait to the model you would like to make searchable. This trait will register a model observer to keep the model in sync with your search driver:

    <?php

    namespace App;

    use Gtk\Larasearch\Searchable;
    use Illuminate\Database\Eloquent\Model;

    class Post extends Model
    {
        use Searchable;
    }

## Searching with Elastic

When using the Elastic driver, you should configure your Elastic `index` and `hosts` credentials in your `config/larasearch.php` configuration file. Once your credentials have been configured, you will also need to install the Elastic PHP SDK via the Composer package manager:

    composer require elasticsearch/elasticsearch
    
You may begin searching a model using the `search` method. The search method accepts an array or a raw JSON string that will be used to search your models. Check [Elastic document](https://www.elastic.co/guide/en/elasticsearch/client/php-api/current/_search_operations.html) for more information. You should then chain the `get` method onto the search query to retrieve the Eloquent models that match the given search query:

    $orders = App\Order::search(['query' => ['match' => ['title' => 'Star Trek']]])->get();

Since Larasearch searches return a collection of Eloquent models, you may even return the results directly from a route or controller and they will automatically be converted to JSON:

    use Illuminate\Http\Request;

    Route::get('/search', function (Request $request) {
        return App\Order::search($request->search)->get();
    });
    
## License

Larasearch is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
