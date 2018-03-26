laravel-statistics
=================
[![Laravel 5](https://img.shields.io/badge/Laravel-5-orange.svg?style=flat-square)](http://laravel.com)
[![License](http://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](https://tldrlegal.com/license/mit-license)

Пакет для вывода статистики посетителей по их IP адресам для сайта/приложения на Laravel-5.

Особенности и преимущества данного пакета:

*	Пакет не использует внешние сервисы, данные хранятся в отдельной таблице базы данных.
*	Статистика формируется на основе уникальных IP адресов посетителей сайта/приложения.
*	Используется функция для отсеивания из данных статистики поисковых ботов.
*	Есть возможность добавления IP, которые не нужны в статистике в черный спискок.
*	Удобная фильтрация вывода результатов статистики (за день, период, по-определенному IP).


Какая информация выводится по каждому отдельному посетителю:
*	Его уникальный IP адрес с возможностью получения информации о его местонахождении (страна-регион-город).
*	URL просматриваемой страницы и количество переходов.
*	Время посещения определенной страницы.


  
Установка
------------------
* Установка пакета с помощью Composer.

```
composer require klisl/laravel-statistics
```

* Если версия Laravel меньше чем 5.5 - добавьте в файл `config/app.php` вашего проекта в конец массива `providers` строку:

```php
Klisl\Statistics\StatisticsServiceProvider::class,
```
Для версии >=5.5 данный шаг можно пропустить.


* После этого выполните в консоли команду публикации нужных ресурсов:
```
php artisan vendor:publish --provider="Klisl\Statistics\StatisticsServiceProvider"
```

* Выполнить миграцию для создания нужной таблицы в базе данных (консоль):
```
php artisan migrate
```

* Указать названия маршрутов (обычно из файла routes\web.php) по которым должна собираться статистика в файле config\statistics.php
Если маршрут обрабатывает разные типы запросов, статистика будет собирается, только для типа GET.


* Установить пароль к странице статистики или вход только для аутентифицированных пользователей в файле config\statistics.php.



Использование
-------------

Для включения механизма сбора статистики, необходимо, предварительно добавить названия маршрутов по которым будут собираться данные 
в файл config\statistics.php в массив 'name_route'. Например маршрут выводящий список постов:
```
Route::get('/posts', ['uses' => 'PostController@index'])->name('posts');
```
Маршрут отвечающий за вывод страницы контактов:
```
Route::get('/contact',['uses' =>'ContactController@show'])->name('contact');
```

В данном примере, получится:
```
'name_route' => ['posts','contact']
```

Для перехода на страницу статистики наберите:
**ВАШ САЙТ/statistics**

Откроется форма для входа на страницу с вводом пароля или страница аутентификации (в зависимости от настроек).
После ввода правильных данных, откроется сама страница статистики с формами для фильтрации.

При тестировании на локальном компьютере, в статистику попадет IP 127.0.0.1. 
После начала использования пакета на хостинге, необходимо будет добавить свой IP в черный список, чтобы он не выводился в статистике.

При необходимости (если часы посещения будут не совпадать) установите нужное значение временной зоны в файле
config\app.php, например:
```
'timezone' => 'Europe/Kiev',
```


![enter image description here](http://klisl.com/frontend/web/images/external/lar_stat3.jpg)


Мой блог: [klisl.com](http://klisl.com)  