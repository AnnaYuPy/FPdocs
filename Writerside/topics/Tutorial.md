# Запуск Faceplate

Для работы с Faceplate, должен быть обязательно запущен сервер. Локальное имя сервера (узла, ноды) должно быть прописано в /etc/hosts.

Запуск сервера производится через запуск файла:

Для Linux:
```bash
<путь_к_Faceplate>/faceplate/bin/faceplate - среда Linux
```

## Запуск Faceplate {id = fp1}
Далее для запуска в терминале директории, в которой находится Faceplate, необходимо выполнить одну из следующих команд:

./faceplate console - запуск в консоли.
Эта команда предполагает, что можно запустить сборку с интерактивной оболочкой, т.е. можно вводить команды или взаимодействовать с Faceplate через интерфейс командной строки.

./faceplate foreground - запуск на переднем плане. Эта команда запускает сборку на переднем плане и направляет вывод в стандартный поток вывода (stdout).
В режиме foreground необходимо учитывать, что логи пишутся в syslog и необходимо уменьшить размер журнала journalctl --vacuum-size=100M либо # journalctl --vacuum-time=7d

./faceplate daemon - запуск в фоновом режиме. При использовании этой команды сервер будет работать в фоновом режиме без привязки к текущему терминалу.

## Останов Faceplate {id = fp2}
Для останова Faceplate необходимо выполнить в терминале директории, в которой располагается запущенный Faceplate, одно из следующих действий:

Для Linux:
```bash
<путь_к_Faceplate>/faceplate/bin/faceplate - среда Linux
```

Останов консольного режима осуществляется комбинацией клавиш Сtrl+c и выбора литеры “А” (abort) для корректного прерывания процесса.

./faceplate stop - останов сервера, если произведен запуск командой console, foreground, daemon.

Для подключения к узлу необходимо выполнить в терминале директории, в которой располагается запущенный Faceplate, следующую команду:

./faceplate daemon_attach - подключение к узлу, запущенному как daemon

./faceplate remote_console - подключение удаленной оболочки к работающему узлу

ulimit -n 400000

env ATTACH_TO=fp@master.fp - режим подключения и т.д.

env FORCE=true - запуск в случае, если система не отвечает, и если известно, какой сервер актуальный.

RUNTIME=false - запуск со стоп run-time принудительно.
