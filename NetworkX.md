```markdown
# Cheatsheet по NetworkX

**NetworkX** — это библиотека Python для создания, анализа и визуализации графов и сетей. Она предоставляет множество инструментов для работы с графами, включая их создание, модификацию, анализ и визуализацию.

## Установка

```bash
pip install networkx
```

## Основные понятия

- **Граф**: коллекция узлов (вершин) и рёбер (связей) между ними.
- **Направленные и ненаправленные графы**: в направленных графах рёбра имеют направление, а в ненаправленных — не имеют.
- **Вес рёбер**: рёбра могут иметь веса или другие атрибуты.

## Создание графов

### 1. Создание пустого графа

```python
import networkx as nx

# Ненаправленный граф
G = nx.Graph()

# Направленный граф
D = nx.DiGraph()
```

### 2. Добавление узлов и рёбер

```python
# Добавление узлов
G.add_node(1)
G.add_nodes_from([2, 3])

# Добавление рёбер
G.add_edge(1, 2)
G.add_edges_from([(2, 3), (3, 1)])

# Добавление с атрибутами
G.add_edge(1, 2, weight=4.2)
```

### 3. Проверка наличия рёбер и узлов

```python
# Проверка наличия узла
print(G.has_node(1))  # True

# Проверка наличия ребра
print(G.has_edge(1, 2))  # True
```

## Извлечение данных из графа

### 1. Получение рёбер и узлов

```python
# Получение всех узлов
nodes = G.nodes()

# Получение всех рёбер
edges = G.edges()

# Получение рёбер с атрибутами
edges_with_attr = G.edges(data=True)
```

### 2. Доступ к атрибутам рёбер и узлов

```python
# Атрибуты рёбер
weight = G[1][2]["weight"]
print(weight)

# Атрибуты узлов
G.nodes[1]["label"] = "Start"
```

## Визуализация графа

### 1. Основная визуализация

```python
import matplotlib.pyplot as plt

# Построение графика
nx.draw(G, with_labels=True, node_color="skyblue", font_weight="bold")
plt.show()
```

### 2. Визуализация с настройками

```python
# Настройка визуализации (например, цвет и размер узлов)
options = {"node_color": "orange", "node_size": 500, "font_size": 10}
nx.draw(G, **options)
plt.show()
```

## Анализ графов

### 1. Статистика графа

```python
# Количество узлов и рёбер
print("Количество узлов:", G.number_of_nodes())
print("Количество рёбер:", G.number_of_edges())

# Степени узлов (сколько рёбер связано с узлом)
degrees = dict(G.degree())
print(degrees)
```

### 2. Поиск кратчайшего пути

```python
# Кратчайший путь между узлами
shortest_path = nx.shortest_path(G, source=1, target=3)
print("Кратчайший путь:", shortest_path)

# Кратчайший путь с учетом весов
shortest_path_weight = nx.shortest_path_length(G, source=1, target=3, weight="weight")
print("Кратчайший путь по весу:", shortest_path_weight)
```

### 3. Центральность узлов

```python
# Центральность по степени
degree_centrality = nx.degree_centrality(G)
print("Центральность по степени:", degree_centrality)

# Центральность по близости
closeness_centrality = nx.closeness_centrality(G)
print("Центральность по близости:", closeness_centrality)

# Центральность по междуцентровой позиции
betweenness_centrality = nx.betweenness_centrality(G)
print("Центральность по междуцентровой позиции:", betweenness_centrality)
```

### 4. Составление компонент связности

```python
# Компоненты связности
components = list(nx.connected_components(G))
print("Компоненты связности:", components)
```

### 5. Детектор сообществ (кластеризация)

```python
# Использование алгоритма для поиска сообществ
from networkx.algorithms.community import greedy_modularity_communities

communities = list(greedy_modularity_communities(G))
print("Обнаруженные сообщества:", communities)
```

## Работы с направленными графами

### 1. Степени входа и выхода

```python
# Степени входа и выхода для направленного графа
in_degrees = D.in_degree()
out_degrees = D.out_degree()
print("Степени входа:", in_degrees)
print("Степени выхода:", out_degrees)
```

### 2. Поиск цепочек

```python
# Поиск цепочки в направленном графе
chain = nx.shortest_path(D, source="A", target="B")
print("Цепочка:", chain)
```

## Работа с большими графами

### 1. Построение графа из данных

```python
import pandas as pd

# Построение графа из данных
edges_data = pd.DataFrame({
    "Source": ["A", "B", "C", "D"],
    "Target": ["B", "C", "D", "A"]
})

G_from_data = nx.from_pandas_edgelist(edges_data, "Source", "Target")
print("Граф из данных:", G_from_data.edges())
```

### 2. Генерация случайных графов

```python
# Генерация случайного графа (например, граф с 10 узлами и 15 рёбрами)
random_graph = nx.gnm_random_graph(10, 15)
nx.draw(random_graph, with_labels=True)
plt.show()
```

## Полезные ссылки

- [Документация NetworkX](https://networkx.github.io/documentation/stable/)
- [Примеры использования](https://networkx.github.io/documentation/stable/auto_examples/index.html)

---

Этот cheatsheet поможет вам быстро начать работать с библиотекой **NetworkX** для создания и анализа графов, а также визуализировать и анализировать сети, что полезно для задач OSINT, таких как анализ связей между объектами и поиска аномалий.
```