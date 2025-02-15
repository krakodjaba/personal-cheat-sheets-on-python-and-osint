```markdown
# Cheatsheet по Whois и DNS

**Whois** и **DNS** (Domain Name System) — это два инструмента для работы с доменными именами, которые помогают получать информацию о доменах, их владельцах, IP-адресах и настройках.

## Установка библиотек

Для работы с Whois и DNS в Python можно использовать библиотеки:
- **`whois`** для получения данных Whois.
- **`dnspython`** для работы с DNS-записями.

```bash
pip install python-whois dnspython
```

## Работа с Whois

### 1. Получение информации о домене

Для получения информации о домене (например, владелец, дата регистрации, серверы имен) используем библиотеку `whois`.

```python
import whois

# Получение информации о домене
domain = whois.whois("example.com")

# Вывод информации
print(domain)
```

### 2. Пример использования

```python
import whois

# Проверка информации о домене
domain_info = whois.whois("example.com")
print(f"Домен: {domain_info.domain_name}")
print(f"Дата регистрации: {domain_info.creation_date}")
print(f"Дата истечения: {domain_info.expiration_date}")
print(f"DNS-серверы: {domain_info.name_servers}")
```

### 3. Извлечение специфичных данных

```python
# Получение только даты регистрации
creation_date = domain_info.creation_date
print(f"Дата регистрации: {creation_date}")

# Получение DNS серверов
name_servers = domain_info.name_servers
print(f"DNS-серверы: {name_servers}")
```

### 4. Обработка ошибок

Иногда информация о домене может отсутствовать, поэтому важно добавить обработку ошибок.

```python
import whois

try:
    domain_info = whois.whois("nonexistentdomain.xyz")
    print(domain_info)
except Exception as e:
    print(f"Ошибка получения данных Whois: {e}")
```

## Работа с DNS

### 1. Получение записей DNS с использованием `dnspython`

Библиотека `dnspython` позволяет получать различные записи DNS (например, A, MX, NS).

```python
import dns.resolver

# Получение A-записей (IP-адреса)
answers = dns.resolver.resolve("example.com", "A")
for rdata in answers:
    print(f"IP-адрес: {rdata.to_text()}")
```

### 2. Получение MX-записей (почтовые серверы)

```python
# Получение MX-записей (почтовые серверы)
mx_records = dns.resolver.resolve("example.com", "MX")
for mx in mx_records:
    print(f"MX-сервер: {mx.exchange}, Приоритет: {mx.preference}")
```

### 3. Получение NS-записей (DNS-серверы)

```python
# Получение NS-записей (DNS-серверы)
ns_records = dns.resolver.resolve("example.com", "NS")
for ns in ns_records:
    print(f"DNS-сервер: {ns.to_text()}")
```

### 4. Получение TXT-записей (текстовые записи, например, SPF)

```python
# Получение TXT-записей (используется для SPF)
txt_records = dns.resolver.resolve("example.com", "TXT")
for txt in txt_records:
    print(f"TXT-запись: {txt.to_text()}")
```

### 5. Получение SOA-записи (информация о первичном сервере)

```python
# Получение SOA-записи (Start of Authority)
soa_records = dns.resolver.resolve("example.com", "SOA")
for soa in soa_records:
    print(f"SOA-запись: {soa.mname} {soa.rname} {soa.serial} {soa.refresh} {soa.retry} {soa.expire} {soa.minimum}")
```

### 6. Использование DNS-серверов

Можно указать специфические DNS-серверы для запроса, если вам нужно использовать не системные DNS-серверы.

```python
# Указание конкретного DNS-сервера
resolver = dns.resolver.Resolver()
resolver.nameservers = ['8.8.8.8']  # Google DNS
answers = resolver.resolve("example.com", "A")
for rdata in answers:
    print(f"IP-адрес: {rdata.to_text()}")
```

## Дополнительные возможности

### 1. Получение информации о нескольких доменах

Можно обрабатывать список доменов и получать информацию по каждому.

```python
domains = ["example.com", "google.com", "yahoo.com"]

for domain in domains:
    try:
        domain_info = whois.whois(domain)
        print(f"Информация о {domain}:")
        print(f"Дата регистрации: {domain_info.creation_date}")
        print(f"Дата истечения: {domain_info.expiration_date}")
    except Exception as e:
        print(f"Ошибка при получении данных для {domain}: {e}")
```

### 2. Запрос DNS-записей для нескольких доменов

```python
domains = ["example.com", "google.com", "yahoo.com"]

for domain in domains:
    try:
        a_records = dns.resolver.resolve(domain, "A")
        print(f"A-записи для {domain}:")
        for rdata in a_records:
            print(rdata.to_text())
    except Exception as e:
        print(f"Ошибка при получении A-записей для {domain}: {e}")
```

## Полезные ссылки

- [Документация Whois](https://pypi.org/project/python-whois/)
- [Документация dnspython](https://dnspython.readthedocs.io/en/latest/)
- [WHOIS Database Information](https://whois.icann.org/en)