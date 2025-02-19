# Salvager

[![Build Status](https://travis-ci.com/kawax/salvager.svg?branch=master)](https://travis-ci.com/kawax/salvager)
[![Maintainability](https://api.codeclimate.com/v1/badges/5fca43bab4a3c98d4d1a/maintainability)](https://codeclimate.com/github/kawax/salvager/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/5fca43bab4a3c98d4d1a/test_coverage)](https://codeclimate.com/github/kawax/salvager/test_coverage)

WebCrawler.

Build with Laravel Dusk and Symfony DomCrawler.

- https://github.com/laravel/dusk
- https://github.com/symfony/dom-crawler

## Requirements
- PHP >= 7.3
- Latest Chrome. Linux, Mac, Windows.

## Installation

```
composer require revolution/salvager
```

### Laravel config(Option)
```
php artisan vendor:publish --provider="Revolution\Salvager\Providers\SalvagerServiceProvider"
```

### Lumen, Laravel Zero
- ServiceProvider: `Revolution\Salvager\Providers\SalvagerServiceProvider::class,`
- Facade: `Revolution\Salvager\Facades\Salvager::class,`

## Plain PHP Demo by Docker

```
git clone https://github.com/kawax/salvager.git salvager && cd $_

docker-compose run --rm composer install

docker-compose run --rm example google.php
//Show Google search results.
//Store screenshot at ./examples/screenshots/
```

## Usage(Laravel)

You can use the `Salvager` Facade anywhere. Controller, Command, Job...

```php
use Laravel\Dusk\Browser;
use Symfony\Component\DomCrawler\Crawler;

use Revolution\Salvager\Facades\Salvager;

class SalvagerController
{
    public function __invoke()
    {
        Salvager::browse(function (Browser $browser) use (&$crawler) {
            $crawler = $browser->visit('https://www.google.com/')
                               ->keys('input[name=q]', 'Laravel', '{enter}')
                               ->screenshot('google-laravel')
                               ->crawler();
        });

        /**
         * @var Crawler $crawler
         */
        $crawler->filter('.r')->each(function (Crawler $node) {
            dump($node->filter('h3')->text());
            dump($node->filter('a')->attr('href'));
        });
    }
}
```

https://github.com/kawax/salvager-project

## Develop
```
docker-compose run --rm phpunit
```

## LICENSE
MIT  
