```markdown
# Cheatsheet по Основам Python

**Python** — это высокоуровневый, интерпретируемый язык программирования, который используется для различных целей, от разработки веб-приложений до анализа данных. Этот cheatsheet охватывает основные темы, которые помогут вам быстро освоиться с Python.

## Установка Python

Для установки Python используйте [официальный сайт Python](https://www.python.org/downloads/), или установите с помощью менеджера пакетов, например, для Ubuntu:

```bash
sudo apt-get install python3
```

### Проверка версии Python

```bash
python --version  # Или python3 --version
```

## Основы синтаксиса

### 1. Переменные и типы данных

```python
# Числа
x = 5  # Целое число
y = 3.14  # Число с плавающей запятой

# Строки
name = "Alice"

# Логические значения
is_active = True
is_verified = False

# Списки
numbers = [1, 2, 3, 4, 5]

# Кортежи (неизменяемые списки)
coordinates = (10, 20)

# Множества
unique_numbers = {1, 2, 3, 4}

# Словари (словарь ключ-значение)
person = {"name": "Alice", "age": 25}
```

### 2. Операторы

```python
# Арифметические операторы
x = 10
y = 3
result = x + y  # Сложение
result = x - y  # Вычитание
result = x * y  # Умножение
result = x / y  # Деление
result = x // y  # Целочисленное деление
result = x % y  # Остаток от деления
result = x ** y  # Возведение в степень

# Логические операторы
a = True
b = False
result = a and b  # Логическое И
result = a or b   # Логическое ИЛИ
result = not a    # Логическое НЕ

# Операторы сравнения
result = x == y  # Равно
result = x != y  # Не равно
result = x > y   # Больше
result = x < y   # Меньше
result = x >= y  # Больше или равно
result = x <= y  # Меньше или равно
```

### 3. Управление потоком

#### 3.1 Условия

```python
x = 10
if x > 5:
    print("x больше 5")
elif x == 5:
    print("x равно 5")
else:
    print("x меньше 5")
```

#### 3.2 Циклы

```python
# Цикл for
for i in range(5):  # От 0 до 4
    print(i)

# Цикл while
count = 0
while count < 5:
    print(count)
    count += 1
```

#### 3.3 Прерывание циклов

```python
# Остановка цикла
for i in range(10):
    if i == 5:
        break  # Выход из цикла
    print(i)

# Пропуск итерации
for i in range(10):
    if i == 5:
        continue  # Пропускаем текущую итерацию
    print(i)
```

## Функции

### 4.1 Определение функции

```python
def greet(name):
    return f"Hello, {name}!"

# Вызов функции
print(greet("Alice"))
```

### 4.2 Параметры и возвращаемые значения

```python
def add(a, b):
    return a + b

result = add(3, 4)
print(result)  # Выводит 7
```

### 4.3 Аргументы по умолчанию

```python
def greet(name="Guest"):
    return f"Hello, {name}!"

print(greet())  # Выводит "Hello, Guest"
print(greet("Bob"))  # Выводит "Hello, Bob"
```

### 4.4 Лямбда-функции (анонимные функции)

```python
# Лямбда-функция для сложения двух чисел
add = lambda x, y: x + y
print(add(3, 4))  # Выводит 7
```

## Строки

### 5.1 Основы работы со строками

```python
text = "Hello, World!"

# Доступ к символам строки
print(text[0])  # H
print(text[-1])  # !

# Срезы
print(text[0:5])  # Hello
print(text[7:])   # World!

# Методы строк
print(text.lower())  # hello, world!
print(text.upper())  # HELLO, WORLD!
print(text.replace("Hello", "Hi"))  # Hi, World!
```

### 5.2 Форматирование строк

```python
# Строки с f-строками
name = "Alice"
age = 25
print(f"My name is {name} and I'm {age} years old.")

# Форматирование через метод format()
print("My name is {} and I'm {} years old.".format(name, age))
```

## Списки

### 6.1 Основы работы со списками

```python
fruits = ["apple", "banana", "cherry"]

# Доступ к элементам списка
print(fruits[0])  # apple

# Изменение элементов списка
fruits[1] = "orange"
print(fruits)  # ['apple', 'orange', 'cherry']

# Добавление элементов
fruits.append("kiwi")
print(fruits)  # ['apple', 'orange', 'cherry', 'kiwi']

# Удаление элементов
fruits.remove("orange")
print(fruits)  # ['apple', 'cherry', 'kiwi']
```

### 6.2 Сортировка и фильтрация

```python
numbers = [5, 3, 8, 1, 2]

# Сортировка списка
numbers.sort()
print(numbers)  # [1, 2, 3, 5, 8]

# Фильтрация списка
even_numbers = [x for x in numbers if x % 2 == 0]
print(even_numbers)  # [2, 8]
```

## Словари

### 7.1 Основы работы со словарями

```python
person = {"name": "Alice", "age": 25, "city": "New York"}

# Доступ к значениям по ключу
print(person["name"])  # Alice

# Добавление нового элемента
person["email"] = "alice@example.com"
print(person)

# Изменение значения
person["age"] = 26
print(person)

# Удаление элемента
del person["city"]
print(person)
```

### 7.2 Метод get и проверка наличия ключа

```python
# Использование метода get для безопасного доступа
email = person.get("email", "Not provided")
print(email)

# Проверка наличия ключа
if "name" in person:
    print("Name is present")
```

## Работа с файлами

### 8.1 Чтение и запись в файлы

```python
# Запись в файл
with open("example.txt", "w") as file:
    file.write("Hello, world!")

# Чтение из файла
with open("example.txt", "r") as file:
    content = file.read()
    print(content)  # Hello, world!
```

### 8.2 Чтение файлов построчно

```python
with open("example.txt", "r") as file:
    for line in file:
        print(line.strip())
```

## Исключения

### 9.1 Обработка ошибок с помощью try-except

```python
try:
    x = 10 / 0  # Деление на ноль
except ZeroDivisionError:
    print("Невозможно разделить на ноль")
```

### 9.2 Создание собственных исключений

```python
class CustomError(Exception):
    pass

try:
    raise CustomError("Это пользовательская ошибка")
except CustomError as e:
    print(e)
```

## Полезные ссылки

- [Официальная документация Python](https://docs.python.org/3/)
- [Python для начинающих](https://docs.python.org/3/tutorial/index.html)

---

Этот cheatsheet поможет вам быстро освоиться с основами Python и начать работать с основными структурами данных, управлением потоком и файловыми операциями.