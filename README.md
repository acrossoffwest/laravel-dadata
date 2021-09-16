# Laravel DaData

Пакет работы с сервисом [DaData.ru](https://dadata.ru/?ref=﻿58539), для исправления синтаксических ошибок в информации контактных данных клиентов сайта и вывода подсказок поля форм.

## Установка

Запустить:
```bash
composer require "fomvasss/laravel-dadata"
```
---
__Для Laravel < 5.5:__
Зарегистрировать service-provider в config/app.php:
```php
  Fomvasss\Dadata\DadataServiceProvider::class,
```
Для Lumen добавить в bootstrap/app.php:
```php
$app->withFacades();
```
---
Опубликовать конфиг: 
```bash
php artisan vendor:publish --provider="Fomvasss\Dadata\DadataServiceProvider"
```
Задать токен (и ключ для API стандартизации) в `config/dadata.php` или `.env`
```php
    'token' => env('DADATA_TOKEN', ''),
    'secret' => env('DADATA_SECRET', ''),
```
## Использование

### Сервис подсказок (https://dadata.ru/api/suggest/)
Добавить в клас фасад:
```php
use Fomvasss\Dadata\Facades\DadataSuggest;
```
1. Пример использование метода с параметрами:
    ```php
    $result = DadataSuggest::suggest("address", ["query"=>"Москва", "count"=>2]);
    print_r($result);
    ```
    Первым параметором может быть: `fio, address, party, email, bank`

2. Пример использование [поиска по ИНН или ОГРН](https://dadata.ru/api/find-party/) с параметрами:

    ```php
    $result = DadataSuggest::partyById('5077746329876', ["branch_type"=>"MAIN"]);
    print_r($result);
    ```
    Первым параметором может быть ИНН, ОГРН или Dadata HID

3. Пример использование [API подсказок по адресам](https://dadata.ru/api/suggest/address/):

    ```php
    $result = DadataSuggest::suggestAddress("Москва", 2);
    print_r($result);
   
4. Пример использование [обратное геокодирование (адрес по координатам)](https://dadata.ru/api/geolocate/):

    ```php
    $result = DadataSuggest::geolocateAddress(55.752448, 37.633777);
    print_r($result);
    ```

### Сервис стандартизации (https://dadata.ru/api/clean/)
Добавить в клас фасад:
```php
use Fomvasss\Dadata\Facades\DadataClean;
```
Использовать методы: 
```php
$response = DadataClean::cleanAddress('мск сухонска 11/-89');
$response = DadataClean::cleanPhone('тел 7165219 доб139');
$response = DadataClean::cleanPassport('4509 235857');
$response = DadataClean::cleanName('Срегей владимерович иванов');
$response = DadataClean::cleanEmail('serega@yandex/ru');
$response = DadataClean::cleanDate('24/3/12');
$response = DadataClean::cleanVehicle('форд фокус');
$response = DadataClean::getStatistics();
$response = DadataClean::getStatistics(now()->subDays(6));
print_r($response);
```

### Проверка баланса системи
```php
$response = DadataClean::getBalance();
```

### Получение статистики использования всех сервисов

На текущий день:

```php
$response = DadataClean::getStatistics();
```

На любую другую дату:

```php
$response = DadataClean::getStatistics(now()->subDays(6));
// or
$response = DadataClean::getStatistics('2019-11-01');
```

## Ссылки, документация, API:
- [https://dadata.ru](https://dadata.ru/?ref=﻿58539)
- https://github.com/gietos/dadata
