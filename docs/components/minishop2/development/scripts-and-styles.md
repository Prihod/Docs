# Скрипты и стили

При инициализации miniShop2 на фронтенде сайта регистрируются скрипты и стили, указанные в системных настройках:

* **ms2_frontend_js** - путь к скриптам, по умолчанию `[[+jsUrl]]web/default.js`
* **ms2_frontend_css** - путь к стилям, по умолчанию `[[+cssUrl]]web/default.css`

Эти настройки общие для всех сниппетов и вы можете использовать в их пути следующие плейсхолдеры:

* **assetsUrl** - путь к директории assets miniShop2
* **jsUrl** - путь к директории assets/js miniShop2
* **cssUrl** - - путь к директории assets/css miniShop2

Если настройка **ms2_frontend_js** не пустая, то дополнительно на фронтенде регистрируется переменная **miniShop2Config** с примерно конфигурацией системы:

``` javascript
miniShop2Config = {
    cssUrl: "/assets/components/minishop2/css/web/",
    jsUrl: "/assets/components/minishop2/js/web/",
    actionUrl: "/assets/components/minishop2/action.php",
    ctx: "web",
    close_all_message: "закрыть все",
    price_format: [2,"."," "],
    price_format_no_zeros: true,
    weight_format: [3,"."," "],
    weight_format_no_zeros: true
}
```

Это некоторые системные, которые используются для работы скрипта по умолчанию. Если вы его не подключаете, то их не будет.

Если вы хотите изменить родные скрипты или стили, вам нужно переименовать эти файлы, изменить, и указать новые имена в системных настройках - тогда они не будут перезаписаны при обновлении.

## Функции обратного вызова

При разных операциях на фронтенде сайта могут запускаться javascript функции обратного вызова, то есть **callbacks**.

Они предусмотрены для разных операций корзины и заказа, вот список:

* **Cart** - действия с корзиной
    * **add** - добавление товара в корзину
    * **remove** - удаление товара из корзины
    * **change** - изменение количество товара в корзине
    * **clean** - - очистка корзины (удаление всех добавленных товаров)
* **Order** - действия с заказом
    * **add** - добавление поля в заказ (имя, email, адрес и т.д.)
    * **getcost** - получаение текущей стоимости заказа
    * **getrequired** - получение списка обязательных полей для отправки заказа
    * **submit** - отправка заказа на обработку
    * **clean** - очистка всех полей заказа

У **каждого** из этих действий есть несколько этапов, которые вы можете использовать для своей логики.
Каждый этап представляет собой функцию или массив функций, которые будут выполнены в нужный момент:

* **before** - выполняется перед началом любой операции. Если вы вернёте здесь `false`, то операция будет прервана.
* **ajax** - технический запрос на сервер
    * **done** - запрос завершён успешно, то есть сервер вернул какой-то ответ.
    * **fail** - на сервере возникла какая-то техническая ошибка, например он вернул код 500.
    * **always** - выполняется при любом ответе от сервера.
* **response** - получен ответ от сервера (то есть, ajax вернул done)
    * **success** - сервер говорит, что всё хорошо
    * **error** - возникла логическая ошибка, например, незаполнены необходимые поля заказа

Для добавления callback в массив нужно указать путь к нему, имя и тело функции:

``` javascript
miniShop2.Callbacks.add('Cart.add.before', 'restrict_cart', function() {
    miniShop2.Message.error('Добавление товаров в корзину запрещено!');

    return false;
});
```

А для удаления нужен только путь и имя

``` javascript
miniShop2.Callbacks.remove('Cart.add.before', 'restrict_cart');
```

Обе функции возвращают `true` или `false` в зависимости от результата работы.

## Примеры

2 функции, которые выводят всплывающие окошки с результатом добавления поля в заказ:

``` javascript
miniShop2.Callbacks.add('Order.add.response.success', 'orders_add_ok', function(response) {
    miniShop2.Message.success('Всё хорошо!');
    console.log(response);
});
miniShop2.Callbacks.add('Order.add.response.error', 'orders_add_err', function(response) {
    miniShop2.Message.error('Возникла ошибка :-(');
    console.log(response);
});
```

Логируем все ajax запросы с добавлением товаров в корзину

``` javascript
miniShop2.Callbacks.add('Cart.add.ajax.always', 'ajax_log', function(xhr) {
    console.log(xhr);
});
```