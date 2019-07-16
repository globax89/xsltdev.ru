# Oracle

Работа Node.js с Oracle осуществляется с использованием модуля `node-oracledb`.

## Установка

Самый простой способ установить `node-oracledb` - использовать npm, в репозитории которого хранится уже скомпилированная и настроенная версия модуля.

```
npm install oracledb --save
```

!!! note ""

    При установке модуля через npm для ОС Windows потребуется установка Microsoft Visual C++ 2015.

В некоторых случаях использование в операционной системе уже скомпилированного модуля будет невозможным и потребуется ручная компиляция модуля из исходного кода. В этом случае порядок действий следующий:

- Установить Python версии 2.7;
- Установить компилятор C с поддержкой C++ (подойдет Microsoft Visual C++ 2015);
- Выполнить из директории проекта команду:

```
npm install oracle/node-oracledb.git#v3.1.2
```

!!! node ""

    Для работы в Node.js модуля `node-oracledb` вне зависимости от операционной системы, она обязательно должна содержать клиентскую библиотеку Oracle. Например, для Windows x64 ее можно скачать [здесь](https://www.oracle.com/technetwork/topics/winx64soft-089540.html).

## Подключение

Для подключения к базе данных Oracle используется метод `getConnection()` библиотеки `node-oracledb`, которому обязательно передаются следующие параметры:

- `user` - имя пользователя БД;
- `password` - пароль пользователя БД;
- `connectString` - строка подключения в формате `host/service`, где `host` - хост, где запущена БД, а `service` - имя сервиса экземпляра БД.

```js
let oracledb = require('oracledb')

let connection

oracledb
  .getConnection({
    user: 'user',
    password: 'password',
    connectString: 'localhost/XXXDB3'
  })
  .then(conn => (connection = conn))
  .catch(err => throw err)
```

## Выполнение запросов

Чтобы выполнить запрос к базе, используйте метод `execute()`.

```js
connection
  .execute(`SELECT *  FROM cars`)
  .then(result => console.log(result))
  .catch(err => throw err)
```

Если в запросе вам необходимо использовать переданные с клиента данные, то для безопасности осуществления SQL-инъекции внешние данные передаются в запрос вторым параметром в виде массива.

```js
connection
  .execute(`SELECT *  FROM cars WHERE model = :model`, ['Audi'])
  .then(result => console.log(result))
  .catch(err => throw err)
```

В самом запросе внешние данные определяются с помощью двоеточия и имени параметра. Но их подстановка будет происходить по порядку: для первой переменной в запросе - первое значение из массива и т. д.

Цель данной статьи - дать минимальные знания, которых достаточных для начала использования в Node.js модуля `node-oracledb`. На самом деле он имеет обширное API, позволяющее использовать весь функционал Oracle. Более подробно ознакомиться с возможностями `node-oracledb` можно в [официальной документации](https://oracle.github.io/node-oracledb/doc/api.html#getstarted).