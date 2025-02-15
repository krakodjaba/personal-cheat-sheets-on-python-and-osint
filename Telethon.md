```markdown
# Cheatsheet по Telethon

**Telethon** — это асинхронная библиотека Python для работы с Telegram API. Она позволяет взаимодействовать с Telegram через боты и пользователей, а также собирать и отправлять сообщения, управлять чатовыми группами и получать доступ к каналам.

## Установка

```bash
pip install telethon
```

## Основные компоненты

### 1. Создание сессии

Для использования Telethon вам нужно создать сессию с Telegram. Для этого потребуется **API ID** и **API Hash**, которые можно получить на [официальном сайте Telegram](https://my.telegram.org/auth).

```python
from telethon import TelegramClient

# Ваши данные
api_id = 'YOUR_API_ID'
api_hash = 'YOUR_API_HASH'

# Создание клиента
client = TelegramClient('session_name', api_id, api_hash)
```

### 2. Авторизация

При первом запуске клиента вам нужно будет пройти авторизацию. Это делается через отправку кода подтверждения на ваш телефон, который Telegram отправит вам через SMS или через Telegram.

```python
await client.start()
```

### 3. Получение информации о пользователе

Для получения информации о пользователе (например, о себе или другом пользователе):

```python
# Получение информации о текущем пользователе
me = await client.get_me()
print(me.username)

# Получение информации о другом пользователе по его ID или username
user = await client.get_entity('username_or_user_id')
print(user.username)
```

### 4. Получение сообщений из чатов

Для получения сообщений из чатов или каналов можно использовать методы `iter_messages` или `get_messages`.

```python
# Получение последних сообщений из чата
messages = await client.get_messages('username_or_chat_id', limit=10)
for message in messages:
    print(message.sender_id, message.text)

# Получение всех сообщений из чата
async for message in client.iter_messages('username_or_chat_id'):
    print(message.text)
```

### 5. Отправка сообщений

Чтобы отправить сообщение в чат или пользователю, используйте метод `send_message`:

```python
# Отправка текста пользователю или в чат
await client.send_message('username_or_chat_id', 'Привет!')

# Отправка сообщения с аттракциями (например, с изображением)
await client.send_message('username_or_chat_id', 'Смотрите это изображение:', file='image.jpg')
```

### 6. Работа с чатом

#### 6.1 Присоединение к чату

Для того чтобы присоединиться к публичному чату, используйте метод `join_chat`:

```python
# Присоединение к публичному чату
await client.join_chat('chat_link_or_username')
```

#### 6.2 Создание групп и каналов

Создание групп и каналов с помощью Telethon:

```python
# Создание канала
channel = await client(CreateChannelRequest(
    title='New Channel',
    about='About the channel',
    megagroup=False  # False - обычный канал, True - группа
))

# Создание группы
group = await client(CreateChatRequest(
    users=['username1', 'username2'],
    title='New Group'
))
```

### 7. Получение информации о чате или канале

Чтобы получить информацию о чате или канале, используйте `get_entity`:

```python
# Получение информации о чате или канале
chat = await client.get_entity('username_or_chat_id')
print(chat.title, chat.username)
```

### 8. Работа с пользователями и подписками

#### 8.1 Получение списка участников чата

Чтобы получить участников чата или канала:

```python
# Получение списка участников
participants = await client.get_participants('username_or_chat_id')
for user in participants:
    print(user.id, user.username)
```

#### 8.2 Удаление пользователя из чата

Для удаления участника из группы:

```python
# Удаление участника из чата
await client.kick_participant('username_or_chat_id', 'username_to_remove')
```

### 9. Асинхронные вызовы

`Telethon` использует асинхронный подход, что позволяет эффективно работать с большим количеством запросов. Для работы с асинхронными методами следует использовать ключевое слово `await`.

Пример асинхронной функции:

```python
import asyncio
from telethon import TelegramClient

async def main():
    api_id = 'YOUR_API_ID'
    api_hash = 'YOUR_API_HASH'
    client = TelegramClient('session_name', api_id, api_hash)

    await client.start()
    messages = await client.get_messages('username_or_chat_id', limit=5)
    for message in messages:
        print(message.sender_id, message.text)

    await client.disconnect()

# Запуск асинхронной функции
asyncio.run(main())
```

### 10. Работа с файлами и медиа

Чтобы отправлять или получать изображения, видео и другие медиафайлы:

```python
# Отправка изображения
await client.send_file('username_or_chat_id', 'image.jpg')

# Получение файла
message = await client.get_messages('username_or_chat_id', limit=1)
await message[0].download_media('downloads/')
```

### 11. Обработка исключений

Использование try-except блоков для обработки ошибок:

```python
from telethon.errors import SessionPasswordNeededError

try:
    await client.start()
except SessionPasswordNeededError:
    # Запрос пароля при двухфакторной аутентификации
    await client.start(password='your_2fa_password')
```

## Полезные ссылки

- [Документация Telethon](https://docs.telethon.dev/)
- [GitHub репозиторий Telethon](https://github.com/LonamiWebs/Telethon)
- [Tutorials по Telethon](https://docs.telethon.dev/en/latest/tutorials/)