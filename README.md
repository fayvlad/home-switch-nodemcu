# Home IoT light switch on NodeMCU (ESP8266)
Устройство предназначено для управления освещением либо вентиляцией в квартире.
Каждое устройство размещается в распределительной коробке комнаты, куда приходит питание, провода от ламп либо вентиляторов и от кнопок.
Предполагается, что реле можно управлять как удаленно, так и при клике на обычный выключатель.
Выключатель должен быть не перекидной, а в виде кнопки.

Управлять можно например через Openhab:
https://github.com/ssk181/home-switch-openhab

## Hardware
- NodeMCU
- MCP23017
- Relay's (1 - 8)
- Buttons (1 - 8)
- DHT11 or DHT22

## MQTT-сообщения
Устройство шлет сообщения о каждом действии в MQTT очередь:

- /home/iot/{Device-IP}/online               *- 1 - соединился с очередью MQTT, 0 - разъединился (LWT)*
- /home/iot/{Device-IP}/button/{ButtonIndex} *- 1 - короткое нажатие, 2 - длинное*
- /home/iot/{Device-IP}/relay/{ButtonIndex}  *- ON или OFF*

## HTTP REST API
1. Посмотреть текущее состояние реле:
   **/api/relay?index={1-8}**
   Для запроса текущего статуса реле, возвращает JSON:
   *{index: 1, state: 1, uptime_sec: 723, free_memmory_byte: 6912}*
2. Управление реле:
   **/api/relay?index={0-8}[&set=0-2]**
   Возвращает аналогичный предыдущему JSON.
   Если указано реле с индексом 0, то действие приминится ко всем реле устройства.
   Возможные действия: set=0 - выключение, set=1 - включение, set=2 - инверсия
3. Получение данных по климату:
   **/api/climate**
   Возвращает JSON с температурой и влажностью:
   *{temp: 24.100, humidity: 99.900, uptime_sec: 1619, free_memmory_byte: 6848}*

## Installation
1. Установить на компьютер nodemcu-tool:
   *npm install nodemcu-tool -g*
2. Установить на NodeMCU Espress:
   https://github.com/loicortola/nodemcu-espress
3. Скриптом *./upload.sh* выгрузить данные скрипты на NodeMCU
