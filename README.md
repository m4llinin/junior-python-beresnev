# junior-python-beresnev

## Структура docker-compose.yml

Состоит из 6 контейнеров, которые объеденены одно сетью для связи. В отдельныз контейнерах открыты порты для
прослушивания, в частности nginx открыт порт 80 и 443 для http и https, соответственно. У postgres и redis открыты их
базовые порты для прослушивания. Основной контейнер бота зависит от контейнеров postgres, redis, nginx для того, чтобы
при запуске бота не было ошибок соединения.

## Связь FastApi и Bot

Связь фреймфорка и бота сделал бы через запросы к телеграмм боту, где в запросе указано название контейнера и номер
порта, который открыт для прослушивания

```
from fastapi import FastAPI
import httpx

app = FastAPI()

@app.get("/")
async def read_root():
    return {"Hello": "World"}

@app.post("/send_message")
async def send_message(message: str):
    async with httpx.AsyncClient() as client:
        response = await client.post('http://bot:8080/send_message', json={'message': message})
        return response.json()
```
