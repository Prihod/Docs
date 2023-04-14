# Категория товаров

Категория товаров предназначена для удобного хранения и управления товарами miniShop2.

Технически, это [CRC][0] **msCategory**, который расширяет стандартный класс modResource.
Это позволяет категории загружать свои собственные javascript и css файлы для более удобной работы с товарами.

## Создание категории

Создать новую категорию можно двумя способами:

* Выбрать нужный пункт в контекстном меню дерева ресурсов

![Создание категории - 1](https://file.modx.pro/files/d/8/7/d87edd56ee056286ed8eb4575db6df6c.png)

* Или переключить тип документа при создании обычного ресурса

![Создание категории - 2](https://file.modx.pro/files/c/b/c/cbc1e2f61632967c578cdfc22763ad93.png)

* Тип ресурса можно менять и потом, превращаю категорию в обычный документ и наоборот*

При создании категории сразу видны некоторые отличия от обычного документа:

* Поле "Содержимое" (content) видно только на первой вкладке.
* В это поле сразу прописывается текст, указанный в системной настройке **ms2_category_content_default**.

![Создание категории - 3](https://file.modx.pro/files/0/e/0/0e0fa2e909480f5310381da4ed291552.png)

* Вкладка с настройками перекомпонована, нет полей "Тип содержимого" (content_type) и "Местонахождение содержимого" (content_dispo)
* Параметр "Контейнер" (isfolder) скрыт - все категории обязательно являются контейнерами.
* Вместо него есть переключатель "Скрыть потомков в дереве", который перекрывает их собственные настройки показа в меню.

![Создание категории - 4](https://file.modx.pro/files/5/4/a/54ad024a03e945a7017c06b93edce074.png)

После создания категории страница перезагружается, и вы видите уже панель изменения категории.

## Изменение категории

Здесь отличий куда больше.

### Управление товарами

Первой вкладкой идёт таблица с товарами категории.

В заголовке таблицы кнопки для создания товара, добавления новой категории и очистки корзины с удалёнными ресурсами.
Весь этот функционал есть в дереве ресурсов системы и в таблицу добавлен для удобства при работе в полноэкранном режиме (когда дерево ресурсов свёрнуто).

Также есть поиск по следующим свойствам товаров:

* Если указано целое число, то ищется точное совпадение среди **id** товаров.
* Если нет, то неточное совпадение по полям

* **pagetitle** - название товара
* **longtitle** - расширенное название
* **description** - описание
* **introtext** - вводный текст
* **article** - артикул товара
* **made_in** - страна производства товара
* Название производителя товара (name привязанного msVendor)
* Название категории товаров (pagetitle родительской msCategory)

Если включена системная настройка **ms2_category_show_nested_products** (по умолчанию), то выводятся все вложенные товары на глубину до 10 подкатегорий.
Поиск также идёт с учётом этой настройки, что позволяет, например, зайти в корневую категорию каталога и найти все товары одной подкатегории по её названию.

Отличить прямых потомков категории от вложенных товаров других подкатегорий очень просто - они выделены жирным шрифтом.

![Управление товарами](https://file.modx.pro/files/c/f/d/cfd7aedea1539f18cffb4b7077acbca0.png)

#### Групповые действия

У каждого товара есть список действий в колонке справа. Вы можете выделять сразу несколько строк, используя Shift или Ctrl (Cmd).

Можно:

* открыть товар на сайте, в новом окне
* открыть товар для редактирования, в этом окне (также можно кликнуть на ссылку с название товара)
* сделать копию товара
* опубликовать \ снять с публикации товар
* удалить \ восстановить
* спрятать \ показать товар в дереве ресурсов

#### Сортировка

Выделенные товары можно сортировать перетаскиванием.
Просто выделите один или несколько товаров и перетащите на другой товар - при этом изменится menuindex всех участников этого процесса.

Для правильной сортировки все товары должны быть в одной категории.

#### Перенос в подкатегорию

Если же вы перетаскиваете товар на участника **другой** категории, то он будет туда **перемещён**.
То есть, у него изменится родительская категория, поле parent.

Таким образом можно быстро менять категории вложенных товаров, но это работает, только если в ней уже выводится хотя-бы один товар.

#### Быстрое редактирование

Набор доступных колонок таблицы указывается в системной настройке **ms2_category_grid_fields**.
Большинство из них вы можете быстро редактировать двойным кликом по нужному полю.
На момент написания этой документации, для вывода доступны следующие колонки:

> Свойства ресурса

* **id** - первичный ключ, только для чтения
* **pagetitle** - имя товара как ссылка на его редактирование. Также выводится id товара и название подкатегории, если товар вложенный по отношению к текущей категории.
* **longtitle** - длинное название, можно редактировать как текст
* **description** - описание товара, можно редактировать как текст
* **alias** - псевдоним товара для дружественных url, можно редактировать как текст
* **introtext** - вводный текст, можно редактировать
* **content** - содержимое ресурса, можно редактировать как текст
* **template** - выбор шаблона из выпадающего списка
* **createdby** - выбор пользователя из выпадающего списка
* **createdon** - выбор даты и времени создания ресурса
* **editedby** - выбор пользователя из выпадающего списка
* **editedon** - выбор даты и времени редактирования ресурса
* **deleted** - ресурс отмечен для удаления: да \ нет.
* **deletedon** - выбор даты и времени удаления ресурса
* **deletedby** - выбор пользователя из выпадающего списка
* **published** - ресурс опубликован: да \ нет.
* **publishedon** - дата публикации ресурса
* **publishedby** - выбор пользователя из выпадающего списка
* **menutitle** - можно редактировать как текст
* **menuindex** - целое число с порядковым номером положения ресурса в текущей категории
* **uri** - ЧПУ ссылка на ресурс, можно редактировать как текст
* **uri_override** - ссылка заморожена: да \ нет
* **show_in_tree** - показывать этот товар в дереве ресурсов: да \ нет
* **hidemenu** - не показывать товар при выводе меню на сайте: да \ нет
* **richtext** - отметка о подключении редактора содержимого: да \ нет
* **searchable** - отметка об индексации товара для поиска: да \ нет
* **cacheable** - отметка о кэшировании товара: да \ нет

> Свойства товара

* **new** - отметка о том, что товар новинка: да \ нет
* **favorite** - отметка о том, что товар особенный: да \ нет
* **pupular** - отметка о том, что товар популярный: да \ нет
* **article** - артикул, можно редактировать как текст
* **price** - стоимость товара, число до 2х знаков после запятой
* **old_price** - предыдущая стоимость товара, число до 2х знаков после запятой
* **weight** - вес товара, число до 3х знаков после запятой
* **image** - большое изображение товара
* **thumb** - маленькое изображение товара
* **vendor** - выбор производителя из выпадающего списка
* **vendor_name** - имя производителя, только для чтения
* **made_in** - страна производства товара, можно редактировать как текст

Поля, в которых содержатся массивы значений, такие как **color**, **size** и **tags** в таблице не выводятся.
Вы можете изменить это поведение или добавить свои собственные поля через расширение [системы плагинов товаров][1].

### Опции товаров

Таблица с дополнительными свойствами товара, назначенными категории через [настройки miniShop2][2].
Вы можете добавить уже созданные свойства вручную, или скопировать их из другой категории.

![Опции товаров](https://file.modx.pro/files/b/d/7/bd729e2da9295e635ffe33e1926c1a3c.png)

Из меню действий вы можете:

* включить или отключить свойства товаров
* включить или отключить обязательное требования заполнения свойства товара
* убрать свойство для этой категории

Обязательные свойств выделяются жирным начертанием.
Также доступно быстрое редактирование значений по умолчанию и сортировка свойств перетаскиванием.

[0]: http://rtfm.modx.com/revolution/2.x/developing-in-modx/advanced-development/custom-resource-classes
[1]: /components/minishop2/development/product-plugins
[2]: /components/minishop2/interface/settings