# ecomet_old.js

Файл ecomet_old.js содержит JavaScript API для взаимодействия с базой данных FACEPLATE через WebSocket-соединение. Основные функции включают установление и управление соединениями, аутентификацию пользователей, создание, редактирование, удаление и поиск объектов в базе данных, а также подписку и отписку на изменения данных. Это позволяет разработчикам легко интегрировать и управлять данными Faceplate в режиме реального времени.

ecomet.connect(IP, Port, Protocol, OnOk, OnError, OnClose)
Описание: Устанавливает WebSocket-соединение с указанным сервером.

Параметры:

IP (строка): IP-адрес сервера.
Port (число): Порт для подключения.
Protocol (строка): Протокол для соединения (например, "http" или "https").
OnOk (функция): Колбэк, вызываемый при успешном подключении.
OnError (функция): Колбэк, вызываемый при ошибке подключения.
OnClose (функция): Колбэк, вызываемый при закрытии соединения.
Пример использования:

javascript
Copy code
ecomet.connect('192.168.0.1', 8080, 'http',
function() {
console.log('Соединение установлено');
},
function(error) {
console.error('Ошибка соединения', error);
},
function() {
console.log('Соединение закрыто');
}
);
ecomet.login(Login, Pass, OnOk, OnError, Timeout)
Описание: Выполняет авторизацию пользователя в системе.

Параметры:

Login (строка): Логин пользователя.
Pass (строка): Пароль пользователя.
OnOk (функция): Колбэк, вызываемый при успешной авторизации.
OnError (функция): Колбэк, вызываемый при ошибке авторизации.
Timeout (число, необязательный): Время ожидания ответа (в миллисекундах).
Пример использования:

javascript
Copy code
ecomet.login('admin', 'password123',
function() {
console.log('Авторизация успешна');
},
function(error) {
console.error('Ошибка авторизации', error);
},
5000
);
ecomet.create_object(Fields, OnOk, OnError, Timeout)
Описание: Создает новый объект в базе данных.

Параметры:

Fields (объект): Поля создаваемого объекта.
OnOk (функция): Колбэк, вызываемый при успешном создании объекта.
OnError (функция): Колбэк, вызываемый при ошибке создания объекта.
Timeout (число, необязательный): Время ожидания ответа (в миллисекундах).
Пример использования:

javascript
Copy code
var fields = {
name: 'Sensor1',
type: 'temperature',
location: 'Room1'
};

ecomet.create_object(fields,
function() {
console.log('Объект успешно создан');
},
function(error) {
console.error('Ошибка создания объекта', error);
},
5000
);
ecomet.edit_object(OID, Fields, OnOk, OnError, Timeout)
Описание: Редактирует существующий объект в базе данных.

Параметры:

OID (строка): Идентификатор объекта.
Fields (объект): Поля для редактирования.
OnOk (функция): Колбэк, вызываемый при успешном редактировании объекта.
OnError (функция): Колбэк, вызываемый при ошибке редактирования объекта.
Timeout (число, необязательный): Время ожидания ответа (в миллисекундах).
Пример использования:

javascript
Copy code
var fields = {
name: 'Sensor2',
location: 'Room2'
};

ecomet.edit_object('12345', fields,
function() {
console.log('Объект успешно отредактирован');
},
function(error) {
console.error('Ошибка редактирования объекта', error);
},
5000
);
ecomet.delete_object(OID, OnOk, OnError, Timeout)
Описание: Удаляет объект из базы данных.

Параметры:

OID (строка): Идентификатор объекта.
OnOk (функция): Колбэк, вызываемый при успешном удалении объекта.
OnError (функция): Колбэк, вызываемый при ошибке удаления объекта.
Timeout (число, необязательный): Время ожидания ответа (в миллисекундах).
Пример использования:

javascript
Copy code
ecomet.delete_object('12345',
function() {
console.log('Объект успешно удален');
},
function(error) {
console.error('Ошибка удаления объекта', error);
},
5000
);
ecomet.find(QueryString, OnOk, OnError, Timeout)
Описание: Выполняет поиск объектов в базе данных по заданному запросу.

Параметры:

QueryString (строка): Строка запроса.
OnOk (функция): Колбэк, вызываемый при успешном выполнении запроса.
OnError (функция): Колбэк, вызываемый при ошибке выполнения запроса.
Timeout (число, необязательный): Время ожидания ответа (в миллисекундах).
Пример использования:

javascript
Copy code
ecomet.find('type=temperature',
function(result) {
console.log('Поиск успешен', result);
},
function(error) {
console.error('Ошибка поиска', error);
},
5000
);
ecomet.subscribe(QueryString, OnCreate, OnEdit, OnDelete, OnError)
Описание: Подписывается на изменения объектов в базе данных по заданному запросу.

Параметры:

QueryString (строка): Строка запроса для подписки.
OnCreate (функция): Колбэк, вызываемый при создании объекта.
OnEdit (функция): Колбэк, вызываемый при редактировании объекта.
OnDelete (функция): Колбэк, вызываемый при удалении объекта.
OnError (функция): Колбэк, вызываемый при ошибке подписки.
Пример использования:

javascript
Copy code
ecomet.subscribe('type=temperature',
function(result) {
console.log('Объект создан', result);
},
function(result) {
console.log('Объект отредактирован', result);
},
function(result) {
console.log('Объект удален', result);
},
function(error) {
console.error('Ошибка подписки', error);
}
);
ecomet.unsubscribe(id, OnOk, OnError)
Описание: Отменяет подписку на изменения объектов в базе данных.

Параметры:

id (число): Идентификатор подписки.
OnOk (функция): Колбэк, вызываемый при успешной отмене подписки.
OnError (функция): Колбэк, вызываемый при ошибке отмены подписки.
Пример использования:

javascript
Copy code
ecomet.unsubscribe(1,
function() {
console.log('Подписка успешно отменена');
},
function(error) {
console.error('Ошибка отмены подписки', error);
}
);
ecomet.application(module, efunction, function_params, OnOk, OnError, Timeout)
Описание: Выполняет вызов функции определенного модуля в системе.

Параметры:

module (строка): Имя модуля.
efunction (строка): Имя функции.
function_params (объект): Параметры функции.
OnOk (функция): Колбэк, вызываемый при успешном выполнении функции.
OnError (функция): Колбэк, вызываемый при ошибке выполнения функции.
Timeout (число, необязательный): Время ожидания ответа (в миллисекундах).
Пример использования:

javascript
Copy code
var params = {
param1: 'value1',
param2: 'value2'
};

ecomet.application('module1', 'function1', params,
function(result) {
console.log('Функция успешно выполнена', result);
},
function(error) {
console.error('Ошибка выполнения функции', error);
},
5000
);
