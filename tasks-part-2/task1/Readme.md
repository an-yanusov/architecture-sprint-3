# Эндпоинты API для микросервиса «Управление устройствами»
1. Получение информации об устройстве
Эндпоинт: /api/devices/{deviceId}
Метод: GET
Описание: Получает информацию о конкретном устройстве по его идентификатору.
Формат запроса:

Параметры URL:
deviceId: уникальный идентификатор устройства (обязательный).
Формат ответа:

Структура JSON:
```json
{
  "deviceId": "string",
  "houseId": "string",
  "deviceTypeId": "string",
  "ecosystemId": "string",
  "serialNumber": "string",
  "status": "string",
  "createdAt": "string",
  "updatedAt": "string"
}
```
Коды ответа:

200: Успех
404: Устройство не найдено
500: Ошибка сервера
Примеры:

Запрос:

```
GET /api/devices/12345
```
Ответ:

```json
{
  "deviceId": "12345",
  "houseId": "67890",
  "deviceTypeId": "thermostat",
  "ecosystemId": "ecosystem1",
  "serialNumber": "SN123456789",
  "status": "включено",
  "createdAt": "2023-01-01T12:00:00Z",
  "updatedAt": "2023-01-01T12:00:00Z"
}
```
2. Обновление состояния устройства
Эндпоинт: /api/devices/{deviceId}/status
Метод: PATCH
Описание: Обновляет состояние устройства.
Формат запроса:

Параметры URL:

deviceId: уникальный идентификатор устройства (обязательный).
Тело запроса (JSON):

```json
{
  "status": "string" // Новое состояние устройства (например, «включено», «выключено»)
}
```
Формат ответа:

Структура JSON:
```json
{
  "message": "string"
}
```
Коды ответа:

200: Успех
404: Устройство не найдено
400: Неверный запрос
500: Ошибка сервера
Примеры:

Запрос:

```
PATCH /api/devices/12345/status
{
  "status": "выключено"
}
```
Ответ:

```json
{
  "message": "Статус устройства обновлен."
}
```
3. Отправка команды устройству
Эндпоинт: /api/devices/{deviceId}/commands
Метод: POST
Описание: Отправляет команду на устройство.
Формат запроса:

Параметры URL:

deviceId: уникальный идентификатор устройства (обязательный).
Тело запроса (JSON):

```json
{
  "action": "string", // Действие, которое должно быть выполнено (например, «включить»)
  "parameters": {
    // Дополнительные параметры для выполнения действия (например, например, заданная температура и т.д.)
  }
}
```
Формат ответа:

Структура JSON:
```json
{
  "commandId": "string",
  "status": "string", // Статус команды (например, «ожидается», «выполнено», «ошибка»)
  "createdAt": "string"
}
```
Коды ответа:

201: Команда успешно отправлена
404: Устройство не найдено
400: Неверный запрос
500: Ошибка сервера
Примеры:

Запрос:

```
POST /api/devices/12345/commands
{
  "action": "setTemperature",
  "parameters": {
    "temperature": 22
  }
}
```
Ответ:

```json
{
  "commandId": "cmd-56789",
  "status": "ожидается",
  "createdAt": "2023-01-01T12:00:00Z"
}
```
4. Получение всех устройств в доме
Эндпоинт: /api/houses/{houseId}/devices
Метод: GET
Описание: Получает список всех устройств, связанных с определённым домом.
Формат запроса:

Параметры URL:
houseId: уникальный идентификатор дома (обязательный).
Формат ответа:

Структура JSON:
```json
[
  {
    "deviceId": "string",
    "status": "string",
    "name": "string"
  },
  ...
]
```
Коды ответа:

200: Успех, список устройств возвращен
404: Дом не найден
500: Ошибка сервера
Примеры:

Запрос:

```
GET /api/houses/67890/devices
```
Ответ:

```json
[
  {
    "deviceId": "12345",
    "status": "включено",
    "name": "Термостат"
  },
  {
    "deviceId": "67891",
    "status": "выключено",
    "name": "Смарт-лампочка"
  }
]
```
Документирование API
Для документирования нашего REST API мы можем использовать Swagger/OpenAPI. Вот пример YAML-документации для вышеописанных эндпоинтов:

```yaml
openapi: 3.0.0
info:
  title: Устройства API
  version: 1.0.0
paths:
  /api/devices/{deviceId}:
    get:
      summary: Получение информации об устройстве
      parameters:
        - name: deviceId
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Успех
          content:
            application/json:
              schema:
                type: object
                properties:
                  deviceId:
                    type: string
                  houseId:
                    type: string
                  deviceTypeId:
                    type: string
                  ecosystemId:
                    type: string
                  serialNumber:
                    type: string
                  status:
                    type: string
                  createdAt:
                    type: string
                    format: date-time
                  updatedAt:
                    type: string
                    format: date-time
        '404':
          description: Устройство не найдено
        '500':
          description: Ошибка сервера

  /api/devices/{deviceId}/status:
    patch:
      summary: Обновление состояния устройства
      parameters:
        - name: deviceId
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                status:
                  type: string
      responses:
        '200':
          description: Успех
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
        '404':
          description: Устройство не найдено
        '400':
          description: Неверный запрос
        '500':
          description: Ошибка сервера

  /api/devices/{deviceId}/commands:
    post:
      summary: Отправка команды устройству
      parameters:
        - name: deviceId
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                action:
                  type: string
                parameters:
                  type: object
      responses:
        '201':
          description: Команда успешно отправлена
          content:
            application/json:
              schema:
                type: object
                properties:
                  commandId:
                    type: string
                  status:
                    type: string
                  createdAt:
                    type: string
                    format: date-time
        '404':
          description: Устройство не найдено
        '400':
          description: Неверный запрос
        '500':
          description: Ошибка сервера

  /api/houses/{houseId}/devices:
    get:
      summary: Получение всех устройств в доме
      parameters:
        - name: houseId
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Успех, список устройств возвращен
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    deviceId:
                      type: string
                    status:
                      type: string
                    name:
                      type: string
        '404':
          description: Дом не найден
        '500':
          description: Ошибка сервера
```
