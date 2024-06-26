# ecomet.js

Класс Ecomet (ecomet.js) предоставляет интерфейс на JavaScript для взаимодействия с СУБД Ecomet через WebSocket. Ниже приведен подробный список методов, включая описание их параметров и возвращаемых значений, а также примеры использования.

## Конструктор

### constructor()
: метод для создания и инициализации экземпляра класса Ecomet с начальными свойствами.

Синтаксис:

```shell
constructor()
```

#### Пример: {collapsible="true"}

```shell
const ecomet = new Ecomet();
```

## Методы

### connect()
: метод для подключения к серверу Ecomet, используя указанные IP, порт и протокол.

Синтаксис:

```shell
connect(IP, Port, Protocol, OnOk, OnError, OnClose)
```

Параметры:
```
IP (string): IP-адрес сервера.

Port (number): Номер порта сервера.

Protocol (string): Протокол (например, "http", "https").

OnOk (function): Callback-функция при успешном подключении.

OnError (function): Callback-функция при ошибке подключения.

OnClose (function): Callback-функция при закрытии соединения.
```
Возвращаемое значение:
```
нет.
```
#### Пример: {collapsible="true"}

```shell
const ecomet = new Ecomet();

ecomet.connect(
    "127.0.0.1",
    "8080",
    "ws:", // WebSocket протокол
    () => {
        console.log("Успешное подключение к серверу");
    },
    (error) => {
        console.error("Ошибка подключения:", error);
    },
    () => {
        console.log("Соединение закрыто");
    }
);
```

### forkConnection()
: метод, который создает форк (ответвление) подключение к серверу Ecomet.

Синтаксис:

```shell
forkConnection()
```

Параметры:
```
нет.
```
Возвращаемое значение:
```
Promise<Ecomet>: Обещание, которое разрешается в новый экземпляр 
Ecomet с активным форк-подключением.
```
#### Пример: {collapsible="true"}

```shell
ecomet.forkConnection()
    .then(forkedConnection => {
        console.log("Форк соединения успешно создан");
        // Используйте forkedConnection для дальнейших операций
    })
    .catch(error => {
        console.error("Ошибка создания форка соединения:", error);
    });
```

### connectUrl()
: метод для подключения к серверу Ecomet, используя указанный URL.

Синтаксис:

```shell
connectUrl(URL, OnOk, OnError, OnClose)
```

Параметры:
```
URL (string): WebSocket URL сервера.

OnOk (function): Callback-функция при успешном подключении.

OnError (function): Callback-функция при ошибке подключения.

OnClose (function): Callback-функция при закрытии соединения.
```
Возвращаемое значение:
```
нет.
```
#### Пример: {collapsible="true"}

```shell
const ecomet = new Ecomet();

const serverUrl = "ws://127.0.0.1:8080/websocket"; // URL сервера

ecomet.connectUrl(
    serverUrl,
    () => {
        console.log("Успешное подключение к серверу");

		// Пример выполнения запроса после подключения 
		const queryStatement = "SELECT * FROM objects WHERE type='example'"; 
		ecomet.query( 
			queryStatement, 
			(result) => { 
				console.log("Результаты запроса:", result); 
			}, 
			(error) => { 
				console.error("Ошибка выполнения запроса:", error); 
			}, 
			5000 // Таймаут 5 секунд 
		); 
	}, 
	(error) => { 
		console.error("Ошибка подключения:", error); 
	}, 
	() => { 
		console.log("Соединение закрыто"); 
	} 
);
```

### close()
: метод, который закрывает WebSocket соединение.

Синтаксис:

```shell
close()
```

Параметры:
```
нет.
```
Возвращаемое значение:
```
нет.
```
#### Пример: {collapsible="true"}

```shell
ecomet.close();
console.log("Соединение закрыто");
```

### is_ok()
: метод, который проверяет, открыто ли WebSocket соединение.

Синтаксис:

```shell
is_ok()
```

Параметры:
```
нет.
```
Возвращаемое значение:
```
boolean: Возвращает true, если соединение открыто, иначе false.
```
#### Пример: {collapsible="true"}

```shell
if (ecomet.is_ok()) {
    console.log("Соединение активно");
} else {
    console.log("Соединение не активно");
}
```

### login()
: метод, который выполняет вход в систему с указанными логином и паролем.

Синтаксис:

```shell
login(login, pass, OnOk, OnError, Timeout)
```

Параметры:
```
login (string): Логин пользователя.
pass (string): Пароль пользователя.
OnOk (function): Callback-функция при успешном входе.
OnError (function): Callback-функция при ошибке входа.
Timeout (number): Время ожидания в миллисекундах.
```
Возвращаемое значение:
```
Идентификатор выполненного действия, который можно использовать для 
дополнительных операций, например, для отмены запроса или для 
отслеживаания его состояния. 
```
#### Пример: {collapsible="true"}

```shell
ecomet.login(
    "myLogin",
    "myPassword",
    (result) => {
        console.log("Авторизация успешна:", result);
    },
    (error) => {
        console.error("Ошибка авторизации:", error);
    },
    5000 // Таймаут 5 секунд
);
```

### debug_applyTo()
: метод, который асинхронно применяет отладочные параметры к объекту.

Синтаксис:

```shell
debug_applyTo(fields, oid)
```

Параметры:
```
fields (object): Поля объекта.
oid (string): Идентификатор объекта.
```
Возвращаемое значение:
```
Promise<object>: Обещание, которое разрешается в обновленные поля объекта.
```
#### Пример: {collapsible="true"}

```shell
const fieldsToDebug = { ".folder": "/root/FP/prototypes/example/content" }; 
const oidToDebug = "exampleOid"; 

    ecomet.debug_applyTo(fieldsToDebug, oidToDebug).then(result => { 
	    console.log("Результат обработки данных:", result); 
    }).catch(error => { 
	    console.error("Ошибка обработки данных:", error); 
});
```

### create_object()
: метод для создания нового объекта с указанными полями.

Синтаксис:

```shell
create_object(fields, OnOk, OnError, Timeout)
```

Параметры:
```
fields (object): Поля нового объекта.
OnOk (function): Callback-функция при успешном создании.
OnError (function): Callback-функция при ошибке создания.
Timeout (number): Время ожидания в миллисекундах.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const fields = { 
	name: "New Object", 
	type: "example" 
}; 

ecomet.create_object( 
	fields, 
	(result) => { 
		console.log("Объект успешно создан:", result); 
	}, 
	(error) => { 
		console.error("Ошибка создания объекта:", error); 
	}, 
	5000 // Таймаут 5 секунд 
);
```

### edit_or_create_object()
: метод, который редактирует существующий или создает новый объект с указанными полями.

Синтаксис:

```shell
edit_or_create_object(fields, OnOk, OnError, Timeout)
```

Параметры:
```
fields (object): Поля объекта.
OnOk (function): Callback-функция при успешной операции.
OnError (function): Callback-функция при ошибке операции.
Timeout (number): Время ожидания в миллисекундах.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const editOrCreateFields = { 
	name: "Existing or New Object", 
	type: "example" }; 

ecomet.edit_or_create_object( 
	editOrCreateFields, 
	(result) => { 
		console.log("Объект успешно отредактирован или создан:", result); 
	}, 
	(error) => { 
		console.error("Ошибка редактирования или создания объекта:", error); 
	},
	5000 // Таймаут 5 секунд 
);
```

### edit_object()
: метод для редактирования существующего объекта с указанными полями.

Синтаксис:

```shell
edit_object(oid, fields, OnOk, OnError, Timeout)
```

Параметры:
```
oid (string): Идентификатор объекта.
fields (object): Новые поля объекта.
OnOk (function): Callback-функция при успешной операции.
OnError (function): Callback-функция при ошибке операции.
Timeout (number): Время ожидания в миллисекундах.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const objectId = "123456"; 
const editFields = { 
	name: "Updated Object", 
	type: "example" 
}; 

ecomet.edit_object( 
	objectId, 
	editFields, 
	(result) => { 
		console.log("Объект успешно отредактирован:", result); 
	}, 
	(error) => { 
		console.error("Ошибка редактирования объекта:", error); 
	}, 
	5000 // Таймаут 5 секунд 
);
```

### delete_object()
: метод для удаления объекта с указанным идентификатором.

Синтаксис:

```shell
delete_object(oid, OnOk, OnError, Timeout)
```

Параметры:
```
oid (string): Идентификатор объекта.
OnOk (function): Callback-функция при успешной операции.
OnError (function): Callback-функция при ошибке операции.
Timeout (number): Время ожидания в миллисекундах.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const deleteObjectId = "123456"; 

ecomet.delete_object( 
	deleteObjectId, 
	(result) => { 
		console.log("Объект успешно удален:", result); 
	}, 
	(error) => { 
		console.error("Ошибка удаления объекта:", error); 
	},
	5000 // Таймаут 5 секунд 
);
```

### find()
: метод для поиска объектов по SQL-запросу.

Синтаксис:

```shell
find(statement, OnOk, OnError, Timeout)
```

Параметры:
```
statement (string): SQL-запрос для поиска.
OnOk (function): Callback-функция при успешном выполнении запроса.
OnError (function): Callback-функция при ошибке выполнения запроса.
Timeout (number): Время ожидания в миллисекундах.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const statement = "GET * FROM objects WHERE type='example'";

ecomet.find(
    statement,
    (result) => {
        console.log("Результаты поиска:", result);
    },
    (error) => {
        console.error("Ошибка поиска:", error);
    },
    5000 // Таймаут 5 секунд
);
```

### query()
: метод, который выполняет произвольный SQL-запрос.

Синтаксис:

```shell
query(statement, OnOk, OnError, Timeout)
```

Параметры:
```
statement (string): SQL-запрос.
OnOk (function): Callback-функция при успешном выполнении запроса.
OnError (function): Callback-функция при ошибке выполнения запроса.
Timeout (number): Время ожидания в миллисекундах.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const queryStatement = "SELECT * FROM objects WHERE type='example'";

ecomet.query(
    queryStatement,
    (result) => {
        console.log("Результаты запроса:", result);
    },
    (error) => {
        console.error("Ошибка выполнения запроса:", error);
    },
    5000 // Таймаут 5 секунд
);
```

### get()
: метод выполняет запрос и возвращает результат в удобном формате.

Синтаксис:

```shell
get(statement, OnOk, OnError, Timeout)
```

Параметры:
```
statement (string): SQL-запрос.
OnOk (function): Callback-функция при успешном выполнении запроса.
OnError (function): Callback-функция при ошибке выполнения запроса.
Timeout (number): Время ожидания в миллисекундах.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const getStatement = "GET .oid FROM objects WHERE type='example'";

ecomet.get(
    getStatement,
    (result) => {
        console.log("Полученные данные:", result);
    },
    (error) => {
        console.error("Ошибка получения данных:", error);
    },
    5000 // Таймаут 5 секунд
);
```

### subscribe()
: метод, который подписывается на изменения объектов, соответствующих указанному SQL-запросу.

Синтаксис:

```shell
subscribe(statement, OnCreate, OnUpdate, OnDelete, OnError)
```

Параметры:
```
statement (string): SQL-запрос.
OnCreate (function): Callback-функция при создании объекта.
OnUpdate (function): Callback-функция при обновлении объекта.
OnDelete (function): Callback-функция при удалении объекта.
OnError (function): Callback-функция при ошибке выполнения запроса.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const subscribeStatement = "FROM objects WHERE type='example'";

ecomet.subscribe(

	subscribeStatement, 
	(result) => { 
		console.log("Объект создан:", result); 
	}, 
	(result) => { 
		console.log("Объект обновлен:", result); 
	}, 
	(result) => { 
		console.log("Объект удален:", result); 
	}, 
	(error) => { 
		console.error("Ошибка подписки:", error); 
	} 
);
```

### unsubscribe()
: метод, который отписывается от изменений объектов.

Синтаксис:

```shell
unsubscribe(id, OnOk, OnError)
```

Параметры:
```
id (number): Идентификатор подписки.
OnOk (function): Callback-функция при успешной отписке.
OnError (function): Callback-функция при ошибке отписки.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const subscriptionId = 1; // Идентификатор подписки

ecomet.unsubscribe(
    subscriptionId,
    () => {
        console.log("Подписка успешно отменена");
    },
    (error) => {
        console.error("Ошибка отмены подписки:", error);
    }
);
```

### application()
: метод для вызова функции приложения на сервере.

Синтаксис:

```shell
application(module, method, params, OnOk, OnError, Timeout)
```

Параметры:
```
module (string): Название модуля.
method (string): Название метода.
params (object): Параметры метода.
OnOk (function): Callback-функция при успешном выполнении.
OnError (function): Callback-функция при ошибке выполнения.
Timeout (number): Время ожидания в миллисекундах.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
const module = "exampleModule";
const method = "exampleMethod";
const params = { param1: "value1", param2: "value2" };

ecomet.application(
    module,
    method,
    params,
    (result) => {
        console.log("Метод приложения успешно выполнен:", result);
    },
    (error) => {
        console.error("Ошибка выполнения метода приложения:", error);
    },
    5000 // Таймаут 5 секунд
);
```

## Внутренние методы

### _action()
: метод, который отправляет запрос на сервер. Метод _action используется внутри других методов класса Ecomet для выполнения различных операций.

Синтаксис:

```shell
_action(Type, Params, OnOk, OnError, Timeout, Id)
```

Параметры:
```
Type (string): Тип действия.
Params (object): Параметры действия.
OnOk (function): Callback-функция при успешном выполнении действия.
OnError (function): Callback-функция при ошибке выполнения действия.
Timeout (number): Время ожидания в миллисекундах.
Id (number): Идентификатор действия.
```
Возвращаемое значение:
```
Идентификатор выполненного действия.
```
#### Пример: {collapsible="true"}

```shell
create_object(fields, OnOk, OnError, Timeout) {
    return this._action("create", { fields }, OnOk, OnError, Timeout, -1);
}
```
В этом примере метод create_object вызывает _action с типом действия "create" и параметрами fields.

### _on_receive()
: метод для обработки ответа от сервера. Метод _on_receive в классе Ecomet отвечает за обработку ответов, получаемых от сервера через WebSocket-соединение.

Синтаксис:

```shell
_on_receive(response)
```

Параметры:
```
response (object): Ответ от сервера.
```
Возвращаемое значение:
```
: нет.
```
#### Пример: {collapsible="true"}

```shell
connectUrl(URL, OnOk, OnError, OnClose) {
    URL = URL.replace("https:", "wss:");
    this._URL = URL.replace("http:", "ws:");
    try {
        this._connection = new WebSocket(this._URL);
        this._connection.onopen = OnOk;
        this._connection.onmessage = event => { this._on_receive(event.data) };
        this._connection.onclose = () => {
            this._connection = undefined;
            this._actions = {};
            if (typeof OnClose === "function") OnClose();
        };
    } catch (Error) { OnError(Error); }
}
```
В этом примере метод _on_receive вызывается автоматически при получении сообщения от сервера. Это происходит благодаря настройке обработчика события onmessage в методе connectUrl.

## Статические методы

### to_ecomet1()
: метод, который преобразует результаты запроса в формат Ecomet. Метод to_ecomet1 используется для преобразования результата запроса, полученного от сервера. Он принимает массив, где первый элемент — это заголовки, а остальные элементы — строки данных.

Синтаксис:

```shell
static to_ecomet1([Header, ...Rows], all)
```

Параметры:
```
Header (array): Заголовки полей.
Rows (array): Строки данных.
all (boolean): Флаг, указывающий, следует ли включать все поля.
```
Возвращаемое значение:
```
array: Преобразованные данные.
```
#### Пример: {collapsible="true"}

```shell
const ecomet = new Ecomet();

ecomet.connectUrl(
    "ws://127.0.0.1:8080/websocket",
    () => {
        console.log("Успешное подключение к серверу");

		// Пример выполнения запроса и преобразования данных 
		ecomet.query( 
			"GET * FROM example_table", 
			(result) => { 
				const transformedData = Ecomet.to_ecomet1(result, true);
				console.log("Преобразованные данные:", transformedData); 
			},
			(error) => { 
				console.error("Ошибка выполнения запроса:", error); 
			}, 
			5000 // Таймаут 5 секунд 
		); 
	}, 
	(error) => { 
		console.error("Ошибка подключения:", error); 
	}, 
	() => { 
		console.log("Соединение закрыто"); 
	} 
);
```
Этот пример демонстрирует, как можно использовать метод to_ecomet1 для преобразования данных, полученных от сервера, в удобный для работы формат.