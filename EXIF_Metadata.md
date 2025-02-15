```markdown
# Cheatsheet по EXIF-метаданным

**EXIF (Exchangeable Image File Format)** — это стандарт для хранения метаданных в изображениях, таких как дата и время съемки, информация о камере, геолокация и настройки съемки.

## Установка библиотек

Для работы с EXIF-данными в Python можно использовать библиотеку `Pillow`:

```bash
pip install pillow
```

## Извлечение EXIF-данных из изображения

```python
from PIL import Image
from PIL.ExifTags import TAGS

# Открытие изображения
img = Image.open("image.jpg")

# Извлечение EXIF-данных
exif_data = img._getexif()

# Вывод всех EXIF-данных
if exif_data:
    for tag_id, value in exif_data.items():
        tag = TAGS.get(tag_id, tag_id)
        print(f"{tag}: {value}")
```

### Примечания:
- **`img._getexif()`** — метод для извлечения EXIF-данных из изображения.
- **`TAGS`** — словарь, где ключи — это ID тегов, а значения — их человеческие названия.

## Чтение специфичных данных EXIF

### Дата и время съемки

```python
from PIL import Image
from PIL.ExifTags import TAGS

img = Image.open("image.jpg")
exif_data = img._getexif()

# Получение даты и времени съемки
date_time = exif_data.get(36867)  # 36867 — это тег для DateTimeOriginal
print(f"Дата и время съемки: {date_time}")
```

### Геолокация (широта, долгота)

```python
from PIL import Image
from PIL.ExifTags import TAGS

img = Image.open("image.jpg")
exif_data = img._getexif()

# Получение GPS-координат
gps_info = exif_data.get(34853)  # 34853 — это тег для GPSInfo
if gps_info:
    lat = gps_info[2][0] / gps_info[2][1]  # Широта
    lon = gps_info[4][0] / gps_info[4][1]  # Долгота
    print(f"Широта: {lat}, Долгота: {lon}")
else:
    print("GPS данные отсутствуют.")
```

### Модель камеры и производитель

```python
from PIL import Image
from PIL.ExifTags import TAGS

img = Image.open("image.jpg")
exif_data = img._getexif()

# Получение информации о камере
camera_make = exif_data.get(271)  # 271 — это тег для Make (производитель)
camera_model = exif_data.get(272)  # 272 — это тег для Model (модель)

print(f"Производитель камеры: {camera_make}")
print(f"Модель камеры: {camera_model}")
```

## Полезные теги EXIF

| Тег       | Описание                                      |
|-----------|-----------------------------------------------|
| 271       | Производитель камеры                         |
| 272       | Модель камеры                                |
| 306       | Дата и время съемки                          |
| 34853     | GPS-данные (широта, долгота)                  |
| 33434     | Тип изображения (например, сжатие)            |
| 34855     | Данные о разрешении изображения (XResolution, YResolution) |
| 2710      | Используемый объектив (если доступно)         |
| 37510     | События камеры (например, фокусировка)        |

## Преобразование GPS-координат в десятичный формат

Если в EXIF-данных присутствуют GPS-координаты в формате градусов, минут и секунд (DMS), их можно преобразовать в десятичный формат:

```python
# Преобразование координат из формата DMS в десятичные
def dms_to_decimal(degrees, minutes, seconds):
    return degrees + (minutes / 60.0) + (seconds / 3600.0)

# Пример GPS-данных: 52° 30' 0.11" N, 13° 24' 19.65" E
latitude = dms_to_decimal(52, 30, 0.11)
longitude = dms_to_decimal(13, 24, 19.65)

print(f"Широта: {latitude}, Долгота: {longitude}")
```

## Ошибки и исключения

- Некоторые изображения могут не содержать EXIF-данных, или они могут быть повреждены.
- В таких случаях, важно добавлять обработку ошибок:

```python
try:
    exif_data = img._getexif()
    if exif_data is None:
        print("Нет EXIF-данных в изображении.")
except Exception as e:
    print(f"Ошибка при извлечении EXIF-данных: {e}")
```

## Полезные ссылки

- [Документация по EXIF в Pillow](https://pillow.readthedocs.io/en/stable/handbook/image-file-formats.html#exif)
- [EXIF Tags Dictionary](https://exiv2.org/tags.html)
