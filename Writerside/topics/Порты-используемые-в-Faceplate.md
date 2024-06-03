# Порты, используемые в Faceplate

В Faceplate используется несколько портов для связи и обмена данными. Вот их описание:

<b>1883</b>: предназначен для протокола MQTT (Message Queuing Telemetry Transport), используемого для обмена данными между устройствами IoT и серверами Faceplate.

<b>6660</b>: используется для подключения к серверу лицензий.

<b>7000</b>: используется для соединения с различными компонентами системы по протоколу HTTP.

<b>7443</b>: аналогичен порту 7000, однако обеспечивает защищенное подключение через протокол HTTPS для безопасного веб-доступа.

<b>8000</b>: служит для подключения к базе данных ECOMET.

<b>9000</b>: зарезервирован для соединения с Faceplate Studio и Runtime.

<b>9443</b>: аналогичен порту 9000, обеспечивая защищенное соединение через HTTPS для Faceplate Studio и Runtime.

В пакете Faceplate есть несколько конфигурационных файлов, где при необходимости можно перекофигурировать номера портов:

<b>.../faceplate/releases/3.5/apps/fp.faceplate.config</b>:

```bash
[
   {fp, [
      {system_account,<<"system">>},
      {http,9000},
      {https,9443},
      {cacertfile, "etc/certs/ca-chain.cert.pem"},
      {certfile, "etc/certs/node.cert.pem"},
      {keyfile,  "etc/certs/node.key.pem"},
      {license_server_host,{192,168,1,3}},
      {license_server_port,6660},
   {email, "support@faceplate.io"}
   ]}
].
```

<b>.../faceplate/releases/3.5/apps/fp.ecomet.config</b>:

```bash
[
   {ecomet, [
      {schema_server_cycle,5000},
      {stop_timeout,60000},
      {password,"111111"},
      {supervisor_max_restarts,10},
      {supervisor_max_period,1000},
         {disable_subscriptions, false},
         {router_pool_size, 128},
         {http,[
            {port,8000}
         ]},
      {https,undefined},
      {process_memory_limit,100}
   ]}
].
```