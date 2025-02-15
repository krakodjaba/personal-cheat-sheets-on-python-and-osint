```markdown
# Cheatsheet по Tweepy

**Tweepy** — это библиотека Python для работы с Twitter API. Она позволяет взаимодействовать с Twitter, извлекать твиты, публиковать новые твиты, искать пользователей, а также управлять аккаунтом и его подписчиками.

## Установка

```bash
pip install tweepy
```

## Основные компоненты Tweepy

### 1. Создание приложения в Twitter и получение ключей

1. Перейдите на [Twitter Developer](https://developer.twitter.com/en/apps) и создайте приложение.
2. Получите **API Key**, **API Secret Key**, **Access Token** и **Access Token Secret**.

### 2. Авторизация в Twitter

Для работы с Twitter API необходимо авторизоваться с использованием полученных ключей.

```python
import tweepy

# Ваши ключи API и токены
api_key = 'YOUR_API_KEY'
api_secret_key = 'YOUR_API_SECRET_KEY'
access_token = 'YOUR_ACCESS_TOKEN'
access_token_secret = 'YOUR_ACCESS_TOKEN_SECRET'

# Авторизация в API
auth = tweepy.OAuth1UserHandler(consumer_key=api_key,
                                consumer_secret=api_secret_key,
                                access_token=access_token,
                                access_token_secret=access_token_secret)
api = tweepy.API(auth)
```

### 3. Получение информации о пользователе

```python
# Получение информации о текущем пользователе
user = api.me()
print(user.name, user.screen_name)

# Получение информации о другом пользователе по его username
user_info = api.get_user(screen_name="twitter")
print(user_info.name, user_info.followers_count)
```

### 4. Получение твитов

#### 4.1 Извлечение последних твитов пользователя

```python
# Получение последних 10 твитов пользователя
tweets = api.user_timeline(screen_name='twitter', count=10)
for tweet in tweets:
    print(tweet.text)
```

#### 4.2 Поиск твитов по ключевому слову

```python
# Поиск твитов по ключевому слову
tweets = api.search_tweets(q="python", lang="en", count=10)
for tweet in tweets:
    print(tweet.text)
```

### 5. Публикация твита

```python
# Публикация твита
api.update_status("Hello, world!")
```

### 6. Ответ на твит

```python
# Ответ на твит
tweet_id = 1234567890123456789  # ID твита
api.update_status("This is a reply!", in_reply_to_status_id=tweet_id)
```

### 7. Лайк твита

```python
# Лайк твита
tweet_id = 1234567890123456789  # ID твита
api.create_favorite(tweet_id)
```

### 8. Ретвит

```python
# Ретвит твита
tweet_id = 1234567890123456789  # ID твита
api.retweet(tweet_id)
```

### 9. Следить за пользователем (подписка)

```python
# Подписка на пользователя
api.create_friendship(screen_name="twitter")
```

### 10. Отписка от пользователя

```python
# Отписка от пользователя
api.destroy_friendship(screen_name="twitter")
```

### 11. Получение списка подписчиков

```python
# Получение списка подписчиков
followers = api.followers(screen_name="twitter")
for follower in followers:
    print(follower.name)
```

### 12. Получение списка пользователей, на которых подписан аккаунт

```python
# Получение списка пользователей, на которых подписан аккаунт
friends = api.friends(screen_name="twitter")
for friend in friends:
    print(friend.name)
```

### 13. Приватные сообщения

#### 13.1 Отправка приватного сообщения

```python
# Отправка приватного сообщения
api.send_direct_message(screen_name="twitter", text="Hello, how are you?")
```

#### 13.2 Получение сообщений

```python
# Получение последних сообщений
messages = api.list_direct_messages(count=5)
for message in messages:
    print(message.message_create['message_data']['text'])
```

## Обработка ошибок

### 1. Ошибки в API

```python
try:
    # Попытка сделать запрос
    tweets = api.user_timeline(screen_name='twitter', count=10)
except tweepy.TweepError as e:
    print(f"Ошибка Tweepy: {e}")
```

### 2. Ошибки аутентификации

Если ваша авторизация не прошла:

```python
from tweepy.errors import TweepError

try:
    user = api.me()
except TweepError as e:
    print(f"Ошибка аутентификации: {e}")
```

## Полезные ссылки

- [Документация Tweepy](https://docs.tweepy.org/en/stable/)
- [API Twitter](https://developer.twitter.com/en/docs)
