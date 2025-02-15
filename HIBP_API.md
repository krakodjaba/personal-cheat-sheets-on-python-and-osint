```markdown
# Cheatsheet по HIBP API (Have I Been Pwned API)

**Have I Been Pwned (HIBP)** — это сервис, который позволяет проверить, были ли утечены ваши данные, такие как email-адрес или пароль. HIBP предоставляет API, с помощью которого можно проверить email-адрес или пароль на наличие в известных утечках данных.

## Установка

Для работы с API HIBP используйте библиотеку `requests`:

```bash
pip install requests
```

## Основные операции

### 1. Проверка email на утечку

Для проверки email-адреса используйте эндпоинт `breachedaccount`. Этот эндпоинт возвращает информацию о том, были ли утекшие данные, связанные с данным адресом.

```python
import requests

def check_email_breach(email):
    api_url = f"https://haveibeenpwned.com/api/v3/breachedaccount/{email}"
    headers = {"User-Agent": "OSINT Tool"}  # Указание User-Agent
    response = requests.get(api_url, headers=headers)
    
    if response.status_code == 200:
        return response.json()  # Возвращает список утечек, если есть
    elif response.status_code == 404:
        return "Email не найден в утечках."  # Email не найден
    else:
        return f"Ошибка: {response.status_code}"  # Ошибка запроса

# Пример использования
email = "example@example.com"
breaches = check_email_breach(email)
print(breaches)
```

**Примечание:**  
- **`status_code 200`** — утечка найдена.
- **`status_code 404`** — данных о таком email в утечках нет.

### 2. Проверка пароля на утечку

Для проверки пароля на утечку используется другой API-эндпоинт — **Pwned Passwords**. Вместо передачи полного пароля отправляется его **SHA-1 хэш**.

#### Шаги:
1. Получите хэш пароля.
2. Отправьте запрос к API, используя только первые 5 символов хэша.
3. Получите список паролей с этим префиксом и проверьте, совпадает ли остаток.

```python
import hashlib
import requests

def check_password_breach(password):
    # Преобразование пароля в SHA-1 хэш
    password_hash = hashlib.sha1(password.encode('utf-8')).hexdigest().upper()
    prefix = password_hash[:5]  # Первые 5 символов
    suffix = password_hash[5:]  # Оставшиеся символы

    # Запрос к API
    api_url = f"https://api.pwnedpasswords.com/range/{prefix}"
    response = requests.get(api_url)
    
    if response.status_code == 200:
        # Проверка, есть ли совпадения с оставшейся частью хэша
        for line in response.text.splitlines():
            hash_suffix, count = line.split(":")
            if hash_suffix == suffix:
                return f"Пароль найден в утечках {count} раз"
        return "Пароль безопасен"
    else:
        return f"Ошибка: {response.status_code}"

# Пример использования
password = "password123"
result = check_password_breach(password)
print(result)
```

**Примечание:**  
- **`status_code 200`** — успешно получены результаты.
- **`count`** — количество раз, когда такой пароль встречался в утечках.

### 3. Проверка пароля без отправки полного хэша

Пароль не отправляется целиком, а только его первые 5 символов. Это повышает безопасность, так как сервер не может узнать полный хэш пароля. Однако все возможные совпадения по остаточной части хэша проверяются локально.

```python
import hashlib

def get_password_hash_prefix(password):
    # Генерация SHA-1 хэша
    password_hash = hashlib.sha1(password.encode('utf-8')).hexdigest().upper()
    prefix = password_hash[:5]
    suffix = password_hash[5:]
    
    return prefix, suffix
```

### 4. Получение информации о пароле (некоторые параметры)

Для проверки паролей с помощью API используется метод, при котором предоставляется статистика по паролю в виде количества утечек, но можно расширить запрос, чтобы получать подробности об источниках утечек. Однако для этого потребуется API-ключ для доступа к расширенному функционалу.

## Полезные ссылки

- [Документация HIBP API](https://haveibeenpwned.com/API/Overview)
- [Pwned Passwords Documentation](https://haveibeenpwned.com/API/v3#PwnedPasswords)

## Рекомендации по использованию

- **Не отправляйте полный пароль.** Вместо этого используйте хэш пароля для повышения безопасности.
- **Используйте надежные пароли.** Если ваш пароль был найден в утечке, немедленно смените его на уникальный и сложный.
- **API-ключи и безопасность.** При использовании расширенных возможностей HIBP API используйте API-ключи и не делитесь ими публично.