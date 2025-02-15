```markdown
# Cheatsheet по Geopy и Folium

**Geopy** и **Folium** — это две библиотеки Python, которые помогают работать с геолокацией и картами. Geopy позволяет выполнять геокодирование (поиск координат по адресу) и обратное геокодирование (поиск адреса по координатам), а Folium используется для визуализации данных на интерактивных картах.

## Установка

```bash
pip install geopy folium
```

## Geopy: Работа с геолокацией

### 1. Геокодирование: Получение координат по адресу

Геокодирование позволяет узнать координаты (широту и долготу) по адресу.

```python
from geopy.geocoders import Nominatim

geolocator = Nominatim(user_agent="geoapiExercises")

# Геокодирование адреса
location = geolocator.geocode("Eiffel Tower, Paris")
print(location.address)
print(f"Широта: {location.latitude}, Долгота: {location.longitude}")
```

### 2. Обратное геокодирование: Получение адреса по координатам

Обратное геокодирование позволяет получить адрес по координатам.

```python
# Обратное геокодирование
location = geolocator.reverse("48.8588443, 2.2943506")  # Координаты Эйфелевой башни
print(location.address)
```

### 3. Пример работы с несколькими результатами

```python
# Получение списка возможных адресов
locations = geolocator.geocode("London", exactly_one=False)
for location in locations:
    print(location.address)
```

## Folium: Визуализация данных на карте

**Folium** позволяет создавать интерактивные карты, на которых можно отображать метки, маршруты и другие географические данные.

### 1. Создание базовой карты

```python
import folium

# Создание карты с начальной точкой
map = folium.Map(location=[48.858844, 2.294351], zoom_start=13)  # Координаты Эйфелевой башни

# Сохранение карты в HTML
map.save("map.html")
```

### 2. Добавление маркера на карту

```python
# Добавление маркера на карту
folium.Marker([48.858844, 2.294351], popup="Eiffel Tower").add_to(map)

# Сохранение обновленной карты
map.save("map_with_marker.html")
```

### 3. Добавление круговой области

```python
# Добавление круговой области
folium.Circle([48.858844, 2.294351], radius=1000, color="blue", fill=True, fill_opacity=0.2).add_to(map)

# Сохранение карты с кругом
map.save("map_with_circle.html")
```

### 4. Использование кластеров для маркеров

Если на карте много маркеров, можно использовать кластеризацию, чтобы улучшить визуализацию.

```python
from folium.plugins import MarkerCluster

# Создание кластерного объекта
marker_cluster = MarkerCluster().add_to(map)

# Добавление маркеров в кластер
folium.Marker([48.858844, 2.294351], popup="Marker 1").add_to(marker_cluster)
folium.Marker([48.860611, 2.337644], popup="Marker 2").add_to(marker_cluster)

# Сохранение карты с кластеризацией
map.save("map_with_clusters.html")
```

### 5. Отображение маршрута на карте

```python
# Добавление маршрута
route = folium.PolyLine(locations=[[48.858844, 2.294351], [48.860611, 2.337644]], color="red", weight=2.5, opacity=1).add_to(map)

# Сохранение карты с маршрутом
map.save("map_with_route.html")
```

## Дополнительные возможности

### 1. Сохранение карты в формате PNG

```python
from selenium import webdriver
from folium import plugins

# Преобразование карты в PNG
map = folium.Map(location=[48.858844, 2.294351], zoom_start=13)
map.save('map.html')

driver = webdriver.Chrome()
driver.get("file:///path/to/map.html")
driver.save_screenshot("map.png")
driver.quit()
```

### 2. Использование Heatmap для отображения плотности точек

```python
from folium.plugins import HeatMap

# Пример данных о местоположении
locations = [[48.858844, 2.294351], [48.860611, 2.337644], [48.858222, 2.294445]]

# Добавление тепловой карты
HeatMap(locations).add_to(map)

# Сохранение карты с тепловой картой
map.save("heatmap.html")
```

## Полезные ссылки

- [Документация Geopy](https://geopy.readthedocs.io/en/stable/)
- [Документация Folium](https://python-visualization.github.io/folium/)