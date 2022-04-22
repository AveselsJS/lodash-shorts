# Lodash - ключевые особенности

## Оглавление
- [Lodash - ключевые особенности](#lodash---ключевые-особенности)
  - [Оглавление](#оглавление)
  - [Работа с массивами](#работа-с-массивами)
    - [_.each](#_each)
    - [_.map](#_map)
    - [_.filter](#_filter)
    - [_.find](#_find)
    - [Удаление элементов](#удаление-элементов)
    - [_.some](#_some)
    - [_.every](#_every)
    - [_.orderBy](#_orderby)
    - [_.groupBy](#_groupby)
    - [_.chain](#_chain)
    - [_.head](#_head)
    - [_.tail](#_tail)
    - [_.last](#_last)
    - [_.initial](#_initial)
  - [Работа со строками](#работа-со-строками)
    - [_.toLower](#_tolower)
    - [_.toUpper](#_toupper)
    - [_.split](#_split)
    - [_.join](#_join)
    - [_.capitalize](#_capitalize)
    - [_.camelCase()](#_camelcase)
    - [_.camelCase()](#_camelcase-1)
  - [Продвинутые функции](#продвинутые-функции)
    - [_.random](#_random)
    - [_.uniqueId](#_uniqueid)
    - [Создание одномерных массивов](#создание-одномерных-массивов)
    - [Удаление пустых элементов](#удаление-пустых-элементов)

## Работа с массивами

Рассмотрены методы: each, map, filter, find, without, reject, some, every, orderBy, groupBy, chain, head, tail, last, initial

### _.each

При работе с массивом функция принимает аналогично функции в JavaScript два аргумента:
- item (сам элемент)
- index (индекс элемента)
 
Но _.each может также работать и с объектом, в отличии от JavaScript и принимает также два аргумент:
- item (сам элемент)
- key (индекс элемента)

Файл с примерами [_.each](work_with_array/each.js)

### _.map

Позволяет работать с синтаксическим сахаром и работать с объектами

Файл с примерами [_.map](work_with_array/map.js)

### _.filter

Должен быть построен таким образом, чтобы вернуть boolean. Это значит, что в теле должно быть указано условие. <br/>
Также если ничего не было найдено, то мы получим всегда пустой массив. Другими словами ничего не было найдено. <br/>
Несмотря на то, что на входе может быть объект (Lodash позволяет работать также и с объектами) на выходе всё равно будет массив. <br/>

Файл с примерами [_.filter](work_with_array/filter.js)

### _.find

Тоже самое что и find, но возвращает первый попавшийся объект. Он очень хорош, когда нужно найти что-то по ID или когда необходим просто первый попавшийся элемент.

Файл с примерами [_.find](array_methods/find.js)

### Удаление элементов

Удаление элементов можно реализовать с помощью различных методов, которые можно поделить на те, которые имутабельные и не имутабельные. Простыми словами мутабельные методы это те, которые изменяют наши данные. Это значит, что если у нас есть определенный массив и если мы проводим над ними определенные операции, то соответственно изменяется исходный массив. Когда же применяется не имутабельные методы, то первым делом они создают новый массив, который является копией исходного и уже над этим, новым, массивом проводятся все операции. Имутабельный код нереально сложно дебажить, поэтому необходимо использовать всегда неимутабельный стиль.

Имутабельными методами являются:
- pull. Аналог without, но с имутабельностью.
- remove. Аналог without, но с имутабельностью.

Не имутабельными методами являются:
- without. without не работает с объектами в массиве, поскольку у without простое сравнение и поэтому необходимо его применять при простых сложениях.
- reject. reject - метод противоположный методу filter. Другими словами возвращает все элементы, которые не подходят под условие (в методе filter возвращаются все элементы, которые подходят под условие). reject можно применять на сложные сложениях.

Файл с примерами [_.killed_elements](array_methods/killed_elements.js)

### _.some

some работает похоже на filter, но возвращает boolean, это означает, что если хоть один элемент в массиве выполняет условие, то получаем true иначе false. Это может быть полезно, если нужно проверить наличие хоть чего-то в массиве

Файл с примерами [_.every_and_some](array_methods/every_and_some.js)

### _.every

every работает похоже на filter, но возвращает boolean, это означает, что если каждый элемент в массиве выполняет условие, то получаем true иначе false. Это может быть полезно, если нужно проверить все элементы на соответствие чему либо.

Файл с примерами [_.every_and_some](array_methods/every_and_some.js)

### _.orderBy

orderBy является интитивно понятным методом сортировки по убыванию или возростанию. orderBy это функция принимающая 3 аргумента: 
- массив данных
- второй аргумент это массив значений которые будут сортироватся
- третий аргумент это массив вариаций, как сортировать данные (по убыванию или по возрастанию). 

К примеру если нужно отсортировать все страны по убыванию, а потом каждый город в стране по возрастанию, то это будет
```
_.orderBy(countries, ["country", "city"], ["desc", "asc"])
```

Файл с примерами [_.sort](array_methods/sort.js)

### _.groupBy

В JavaScript не существует метода группировки. Такой костыль можно написать самому или же использовать библиотеку lodash.

Файл с примерами [_.groupBy](array_methods/group.js)

### _.chain

По сути метод chain представляет из себя цепочку методов, где последовательно вкладывая различные методы, на каждом этапе данные обрабатываются и передаются в следующий skope. 
Метод chain всегда начинается словом "сhain" и заканчивается "value", а в середине может быть n-нное количество методов, которые эти данные трансформируют:
```
_.chain([arr]).value()
```

Пример:
```
_.chain(users)
    .filter("isActive")
    .orderBy(["age"])
    .map(function (user) {
      return user.name + " это " + user.age;
    })
    .head()
    .value();
};
```

Используя _.chain не нужно понимать весь код целиком. 

Файл с примерами [_.chain](array_methods/chain.js)

### _.head

Возвращает первый элемент массива, по сути является аналогом arr[0]

Файл с примерами [_.get_part_of_array](array_methods/get_part_of_array.js)

### _.tail

Возвращает все элементы массива кроме первого элемента, по сути является аналогом arr.slice(1).
Нюанс: при работе со строкой, метод возвращает массив с каждым элементом строки, поэтому необходимо использовать метод join, чтобы после использования метода tail на строку - возвращать строку.

Файл с примерами [_.get_part_of_array](array_methods/get_part_of_array.js)

### _.last

Возвращает последний элемент массива, по сути является аналогом arr[arr.length - 1].

Файл с примерами [_.get_part_of_array](array_methods/get_part_of_array.js)

### _.initial

Возвращает все элементы массива кроме последнего элемента, по сути является аналогом arr.slice(0, -1).

Файл с примерами [_.get_part_of_array](array_methods/get_part_of_array.js)

## Работа со строками

### _.toLower

В принципе единственное решение это по сути написание этого метода в цепочке `_.chain`. В остальном аналогично JavaScript методу toLowerCase()

Файл с примерами [_.register_one](transform_string/register_one.js)

### _.toUpper

В принципе единственное решение это по сути написание этого метода в цепочке `_.chain`. В остальном аналогично JavaScript методу toUpperCase()

Файл с примерами [_.register_one](transform_string/register_one.js)

### _.split

В принципе единственное решение это по сути написание этого метода в цепочке `_.chain`. В остальном аналогично JavaScript методу split()

Файл с примерами [_.split_and_join](transform_string/split_and_join.js)

### _.join

В принципе единственное решение это по сути написание этого метода в цепочке `_.chain`. В остальном аналогично JavaScript методу join()

Файл с примерами [_.split_and_join](transform_string/split_and_join.js)

### _.capitalize

Метод делающий первый элемент строки в Upper case, в JavaScript такого метода нет.

Файл с примерами [_.capitalize](transform_string/capitalize.js)

### _.camelCase()

Переводит строку в camelCase формат (toCamelCaseFormat)

Файл с примерами [_.capitalize](transform_string/register_two.js)

### _.camelCase()

Переводит строку в snakeCase формат (to_snake_case_format)

Файл с примерами [_.capitalize](transform_string/register_two.js)

## Продвинутые функции

### _.random

Используется, когда нужно создать случайное значение. При использовании стандартного Math.random генерится большое количество символов после запятой, что не всегда является удобно. 

Файл с примерами [_.randomize](advanced_functions/randomize.js)

### _.uniqueId

Используется если нужно создать уникальный айди, аналог uuid/v4. Под капотом метода находится счётчик в замыкании

Файл с примерами [_.uniqueId](advanced_functions/uniqueId.js)

### Создание одномерных массивов

- Метод flatten делает из вложенных массивов в массив - одномерный массив. flatten делает массив из вложенных массивов. Если вложеность превышает двойку, то массив также останется с вложенными массивами третьей и дальнейшей вложенностей. 
- Метод flattenDeep делает тоже самое что и flatten, но результатом будет всегда одномерный массив.

Файл с примерами [_.plate_methods](advanced_functions/plate_methods.js)

### Удаление пустых элементов

Метод _.compact удаляет из массива все falsy элементы. Falsy элементы - это false, null, undefined.

Файл с примерами [_.empty_elements](advanced_functions/empty_elements.js)

