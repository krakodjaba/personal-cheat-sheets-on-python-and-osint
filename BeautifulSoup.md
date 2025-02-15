# Cheatsheet по BeautifulSoup

**BeautifulSoup** — это библиотека Python для парсинга HTML и XML документов. Она позволяет извлекать данные из веб-страниц в удобном формате.

## Установка

```bash
pip install beautifulsoup4
```

## Импорт библиотеки

```python
from bs4 import BeautifulSoup
```

## Создание объекта BeautifulSoup

```python
soup = BeautifulSoup(html_content, 'html.parser')
```

- **`html_content`** — строка с HTML-кодом или объект ответа `requests`.
- **`'html.parser'`** — парсер для разбора HTML. Также доступны `'lxml'`, `'html5lib'`.

## Основные методы

### Поиск элементов

- **`find(name, attrs, recursive, string, **kwargs)`**: ищет первый элемент, соответствующий условиям.

  ```python
  tag = soup.find('div', class_='content')
  ```

- **`find_all(name, attrs, recursive, string, limit, **kwargs)`**: ищет все элементы, соответствующие условиям.

  ```python
  tags = soup.find_all('a', href=True)
  ```

- **`select(selector)`**: ищет элементы по CSS-селекторам.

  ```python
  items = soup.select('div.content > ul > li')
  ```

### Доступ к содержимому

- **`tag.name`**: имя тега.

  ```python
  print(tag.name)
  ```

- **`tag.attrs`**: словарь атрибутов тега.

  ```python
  print(tag.attrs)
  ```

- **`tag['attribute']`**: значение конкретного атрибута.

  ```python
  href = link_tag['href']
  ```

- **`tag.string`**: строковое содержимое тега (если тег содержит только текст).

  ```python
  print(tag.string)
  ```

- **`tag.text`** или **`tag.get_text()`**: текстовое содержимое тега и всех его потомков.

  ```python
  text = tag.get_text()
  ```

### Навигация по дереву

- **Дочерние элементы**

  - **`tag.contents`**: список непосредственных детей тега.

    ```python
    children = tag.contents
    ```

  - **`tag.children`**: генератор для итерации по дочерним элементам.

    ```python
    for child in tag.children:
        print(child)
    ```

  - **`tag.descendants`**: генератор для итерации по всем потомкам.

    ```python
    for descendant in tag.descendants:
        print(descendant)
    ```

- **Родительские элементы**

  - **`tag.parent`**: непосредственный родитель тега.

    ```python
    parent = tag.parent
    ```

  - **`tag.parents`**: генератор для итерации по всем родителям.

    ```python
    for parent in tag.parents:
        print(parent.name)
    ```

- **Соседние элементы**

  - **`tag.next_sibling`**: следующий соседний тег.

  - **`tag.previous_sibling`**: предыдущий соседний тег.

    ```python
    next_sibling = tag.next_sibling
    previous_sibling = tag.previous_sibling
    ```

## Примеры использования

### Парсинг HTML из строки

```python
html_doc = """
<html>
  <head><title>Пример страницы</title></head>
  <body>
    <h1>Заголовок</h1>
    <p class="description">Это пример страницы.</p>
    <a href="https://example.com" id="link1">Ссылка 1</a>
    <a href="https://example.org" id="link2">Ссылка 2</a>
  </body>
</html>
"""

soup = BeautifulSoup(html_doc, 'html.parser')
```

### Извлечение заголовка страницы

```python
title = soup.title.string
print(title)  # Вывод: Пример страницы
```

### Извлечение текста первого параграфа

```python
description = soup.find('p', class_='description').get_text()
print(description)  # Вывод: Это пример страницы.
```

### Извлечение всех ссылок

```python
links = soup.find_all('a')
for link in links:
    href = link.get('href')
    text = link.get_text()
    print(f'Текст: {text}, URL: {href}')
```

### Использование CSS-селекторов

```python
links = soup.select('a#link1')
for link in links:
    print(link['href'])  # Вывод: https://example.com
```

### Поиск по тексту с использованием регулярных выражений

```python
import re
tags = soup.find_all(text=re.compile('пример'))
for tag in tags:
    print(tag)
```

## Обработка ошибок и проверка наличия элементов

- Проверка на наличие тега

  ```python
  tag = soup.find('div', class_='content')
  if tag:
      # Действия с найденным тегом
      pass
  else:
      print('Тег не найден')
  ```

- Безопасное получение атрибута

  ```python
  href = link.get('href', 'Нет ссылки')
  ```

## Полезные советы

- **Выбор парсера**

  - По умолчанию используется `'html.parser'`, но для более быстрого и гибкого парсинга можно использовать `'lxml'`.

    ```python
    soup = BeautifulSoup(html_content, 'lxml')
    ```

  - Требуется установка дополнительных библиотек:

    ```bash
    pip install lxml
    ```

- **Вывод отформатированного HTML**

  ```python
  print(soup.prettify())
  ```

- **Поиск по нескольким критериям**

  ```python
  tags = soup.find_all('a', attrs={'class': 'external', 'href': re.compile(r'^https://')})
  ```

## Часто используемые шаблоны

### Извлечение всех изображений

```python
images = soup.find_all('img')
for img in images:
    src = img.get('src')
    alt = img.get('alt', 'Нет описания')
    print(f'Изображение: {src}, Описание: {alt}')
```

### Поиск всех таблиц и извлечение данных

```python
tables = soup.find_all('table')
for table in tables:
    rows = table.find_all('tr')
    for row in rows:
        cells = row.find_all(['td', 'th'])
        data = [cell.get_text(strip=True) for cell in cells]
        print(data)
```

### Извлечение текста без тегов

```python
text = soup.get_text(separator='\n', strip=True)
print(text)
```

## Дополнительные возможности

- **Параметр `limit` в `find_all`**: ограничивает количество возвращаемых элементов.

  ```python
  first_three_links = soup.find_all('a', limit=3)
  ```

- **Поиск с использованием функций**

  ```python
  def has_data_id(tag):
      return tag.has_attr('data-id')

  tags_with_data_id = soup.find_all(has_data_id)
  ```

- **Замена и изменение элементов**

  ```python
  for tag in soup.find_all('p'):
      tag.string = 'Новый текст'
  ```

## Полезные ссылки

- [Документация BeautifulSoup (англ.)](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [Примеры использования](https://beautiful-soup-4.readthedocs.io/en/latest/#quick-start)