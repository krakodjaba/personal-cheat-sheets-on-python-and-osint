```markdown
# Cheatsheet по библиотеке Requests

**Requests** — это популярная библиотека Python для работы с HTTP-запросами. Она позволяет легко отправлять HTTP-запросы и обрабатывать ответы.

## Установка

```bash
pip install requests
```

## Основные методы HTTP-запросов

### 1. Отправка GET-запроса

GET-запрос используется для получения данных с сервера.

```python
import requests

response = requests.get("https://example.com")
print(response.status_code)  # Статус код ответа
print(response.text)  # Текстовый ответ
print(response.json())  # Ответ в формате JSON (если есть)
```

### 2. Отправка POST-запроса

POST-запрос используется для отправки данных на сервер.

```python
import requests

data = {'username': 'user', 'password': 'pass'}
response = requests.post("https://example.com/login", data=data)
print(response.status_code)
print(response.text)
```

### 3. Отправка запросов с параметрами (Query Parameters)

```python
import requests

params = {'q': 'python', 'page': 1}
response = requests.get("https://example.com/search", params=params)
print(response.status_code)
print(response.url)  # Параметры URL
print(response.text)
```

### 4. Отправка POST-запроса с JSON

Для отправки данных в формате JSON используется параметр `json`.

```python
import requests

json_data = {'key': 'value'}
response = requests.post("https://example.com/api", json=json_data)
print(response.status_code)
print(response.json())
```

### 5. Заголовки (Headers)

Вы можете передавать заголовки с запросами, используя параметр `headers`.

```python
import requests

headers = {'User-Agent': 'my-app'}
response = requests.get("https://example.com", headers=headers)
print(response.status_code)
print(response.text)
```

### 6. Отправка файлов (Multipart)

Для отправки файлов на сервер используйте параметр `files`.

```python
import requests

files = {'file': open('example.jpg', 'rb')}
response = requests.post("https://example.com/upload", files=files)
print(response.status_code)
```

## Работа с ответом

### 1. Статус код

```python
response = requests.get("https://example.com")
print(response.status_code)  # 200 — успех, 404 — не найдено
```

### 2. Проверка успешности запроса

```python
response = requests.get("https://example.com")
if response.ok:
    print("Запрос успешен")
else:
    print("Ошибка запроса")
```

### 3. Доступ к текстовому содержимому

```python
response = requests.get("https://example.com")
print(response.text)  # Содержимое страницы как текст
```

### 4. Доступ к JSON

Если ответ сервера в формате JSON, можно использовать метод `.json()`:

```python
response = requests.get("https://jsonplaceholder.typicode.com/posts")
json_data = response.json()
print(json_data)
```

### 5. Доступ к заголовкам

```python
response = requests.get("https://example.com")
print(response.headers)  # Заголовки ответа
```

### 6. Доступ к URL запроса

```python
response = requests.get("https://example.com?query=python")
print(response.url)  # Полученный URL с параметрами
```

### 7. Время ответа

```python
response = requests.get("https://example.com")
print(response.elapsed)  # Время ответа на запрос
```

## Обработка ошибок

### 1. Обработка исключений

```python
import requests

try:
    response = requests.get("https://example.com")
    response.raise_for_status()  # Вызывает исключение при ошибке HTTP
except requests.exceptions.HTTPError as errh:
    print(f"HTTP ошибка: {errh}")
except requests.exceptions.ConnectionError as errc:
    print(f"Ошибка подключения: {errc}")
except requests.exceptions.Timeout as errt:
    print(f"Ошибка таймаута: {errt}")
except requests.exceptions.RequestException as err:
    print(f"Ошибка запроса: {err}")
```

### 2. Таймауты

Для установки времени ожидания ответа используйте параметр `timeout`.

```python
response = requests.get("https://example.com", timeout=5)  # 5 секунд на ожидание ответа
```

## Работа с Cookies

### 1. Получение и передача Cookies

```python
# Получение cookies
response = requests.get("https://example.com")
cookies = response.cookies

# Передача cookies
response = requests.get("https://example.com", cookies=cookies)
```

## Продвинутые возможности

### 1. Сессии

Сессии позволяют сохранять параметры и cookies между запросами.

```python
import requests

session = requests.Session()
session.headers.update({'User-Agent': 'my-app'})
response = session.get("https://example.com")
print(response.text)
```

### 2. Прокси-серверы

Вы можете использовать прокси-серверы для запросов.

```python
proxies = {
  'http': 'http://10.10.1.10:3128',
  'https': 'http://10.10.1.10:1080',
}
response = requests.get("https://example.com", proxies=proxies)
print(response.status_code)
```

### 3. Аутентификация

Requests поддерживает базовую аутентификацию и аутентификацию через токены.

- **Базовая аутентификация**

```python
from requests.auth import HTTPBasicAuth

response = requests.get("https://example.com", auth=HTTPBasicAuth('username', 'password'))
print(response.status_code)
```

- **Аутентификация с токеном**

```python
headers = {'Authorization': 'Bearer YOUR_TOKEN'}
response = requests.get("https://example.com", headers=headers)
print(response.status_code)
```

## Полезные ссылки

- [Документация Requests](https://docs.python-requests.org/en/master/)
- [API-запросы и примеры](https://realpython.com/python-requests/)
- [Примеры использования](https://requests.readthedocs.io/en/latest/user/quickstart/)