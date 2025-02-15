```markdown
# Cheatsheet по Scrapy

**Scrapy** — это фреймворк для создания веб-скрейперов и парсинга данных. Scrapy позволяет эффективно собирать и обрабатывать данные с веб-страниц, автоматически управлять запросами, следить за связями между страницами и выводить результаты в различных форматах.

## Установка

```bash
pip install scrapy
```

## Основные компоненты Scrapy

### 1. Создание проекта

Чтобы создать новый проект Scrapy, используйте команду:

```bash
scrapy startproject myproject
```

Это создаст структуру папок с основными файлами:

```
myproject/
    scrapy.cfg
    myproject/
        __init__.py
        items.py
        middlewares.py
        pipelines.py
        settings.py
        spiders/
            __init__.py
```

### 2. Создание паука (Spider)

Паук — это основной компонент Scrapy, который извлекает данные с веб-страниц.

```bash
cd myproject
scrapy genspider example example.com
```

Этот код создаст файл паука, который будет собирать данные с `example.com`.

### 3. Основы написания паука

```python
import scrapy

class ExampleSpider(scrapy.Spider):
    name = 'example'
    start_urls = ['https://example.com']

    def parse(self, response):
        # Извлечение данных
        title = response.css('title::text').get()
        print(title)

        # Переход по ссылкам на другие страницы
        next_page = response.css('a::attr(href)').get()
        if next_page:
            yield response.follow(next_page, self.parse)
```

- **`name`** — уникальное имя для паука.
- **`start_urls`** — начальные страницы для сбора данных.
- **`parse`** — метод для обработки данных и перехода по страницам.

### 4. Извлечение данных с использованием селекторов

Scrapy поддерживает два типа селекторов: **CSS** и **XPath**.

#### CSS-селекторы

```python
title = response.css('title::text').get()
links = response.css('a::attr(href)').getall()
```

#### XPath-селекторы

```python
title = response.xpath('//title/text()').get()
links = response.xpath('//a/@href').getall()
```

### 5. Сохранение данных

Используя команды командной строки, можно сохранить результаты в различных форматах, таких как JSON, CSV или XML.

```bash
scrapy crawl example -o output.json
```

### 6. Работа с параметрами

Вы можете передавать параметры в запросы с помощью аргументов.

```python
def start_requests(self):
    for url in self.start_urls:
        yield scrapy.Request(url, callback=self.parse, meta={'param': 'value'})
```

В дальнейшем вы можете получить доступ к переданным параметрам с помощью `response.meta`.

```python
def parse(self, response):
    param = response.meta['param']
    print(param)
```

### 7. Использование Items для структурирования данных

Items — это структуры для хранения данных, которые будут собираться.

```python
# items.py
import scrapy

class MyItem(scrapy.Item):
    title = scrapy.Field()
    link = scrapy.Field()

# В пауке
def parse(self, response):
    item = MyItem()
    item['title'] = response.css('title::text').get()
    item['link'] = response.url
    yield item
```

### 8. Использование Pipelines для обработки данных

Pipeline — это механизм обработки данных после их извлечения.

#### Пример простой pipeline для сохранения данных в файл:

```python
# pipelines.py
class MyPipeline:
    def process_item(self, item, spider):
        with open('output.txt', 'a') as file:
            file.write(f"{item['title']} - {item['link']}\n")
        return item
```

Чтобы активировать pipeline, добавьте его в настройки `settings.py`:

```python
ITEM_PIPELINES = {
    'myproject.pipelines.MyPipeline': 1,
}
```

### 9. Работа с запросами и пользовательскими заголовками

Вы можете настроить пользовательские заголовки для отправки запросов.

```python
def start_requests(self):
    headers = {
        'User-Agent': 'my-custom-agent',
    }
    for url in self.start_urls:
        yield scrapy.Request(url, headers=headers, callback=self.parse)
```

### 10. Обработка ошибок и исключений

Scrapy позволяет обрабатывать ошибки с помощью метода `errback`, который вызывается в случае ошибки.

```python
def start_requests(self):
    for url in self.start_urls:
        yield scrapy.Request(url, callback=self.parse, errback=self.handle_error)

def handle_error(self, failure):
    self.logger.error(f'Ошибка при обработке страницы: {failure}')
```

### 11. Обработка куки и сессий

Для работы с cookies можно использовать параметр `cookies`.

```python
def start_requests(self):
    cookies = {'sessionid': '123456'}
    for url in self.start_urls:
        yield scrapy.Request(url, cookies=cookies, callback=self.parse)
```

## Полезные команды Scrapy

### 1. Запуск паука

```bash
scrapy crawl example
```

### 2. Создание паука

```bash
scrapy genspider example example.com
```

### 3. Просмотр списка всех доступных пауков

```bash
scrapy list
```

### 4. Проверка правильности настроек

```bash
scrapy check
```

### 5. Отладка паука

```bash
scrapy shell "https://example.com"
```

## Дополнительные возможности Scrapy

### 1. Использование с прокси

Scrapy поддерживает настройку прокси для работы с сайтами.

```python
def start_requests(self):
    proxy = "http://10.10.1.10:3128"
    for url in self.start_urls:
        yield scrapy.Request(url, meta={'proxy': proxy}, callback=self.parse)
```

### 2. Ограничение скорости запросов

Вы можете установить максимальное количество запросов в секунду, чтобы избежать блокировки.

```python
# settings.py
DOWNLOAD_DELAY = 2  # Задержка между запросами (в секундах)
CONCURRENT_REQUESTS = 16  # Максимальное количество параллельных запросов
```

### 3. Работа с CAPTCHA и антиботами

Для обхода CAPTCHA и других антибот-защит часто используется интеграция с сервисами типа `2Captcha` или использование автоматизации браузера (например, через Selenium).

## Полезные ссылки

- [Документация Scrapy](https://docs.scrapy.org/en/latest/)
- [Пример проекта Scrapy](https://github.com/scrapy/scrapy)
- [Обучающие материалы Scrapy](https://docs.scrapy.org/en/latest/tutorial.html)