```markdown
# Cheatsheet по Matplotlib и Seaborn

**Matplotlib** и **Seaborn** — это две мощные библиотеки для визуализации данных в Python. Matplotlib предоставляет базовые возможности для создания графиков, а Seaborn строит на основе Matplotlib, предлагая более красивые и удобные способы визуализации.

## Установка

```bash
pip install matplotlib seaborn
```

## Основные компоненты Matplotlib

### 1. Импорт библиотек

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

### 2. Построение базового графика

**Линейный график**

```python
# Простой линейный график
x = [1, 2, 3, 4, 5]
y = [1, 4, 9, 16, 25]

plt.plot(x, y)
plt.title("Простой линейный график")
plt.xlabel("Ось X")
plt.ylabel("Ось Y")
plt.show()
```

**Гистограмма**

```python
import numpy as np

data = np.random.randn(1000)  # Генерация случайных данных
plt.hist(data, bins=30)
plt.title("Гистограмма")
plt.xlabel("Значения")
plt.ylabel("Частота")
plt.show()
```

**Диаграмма рассеяния (scatter plot)**

```python
x = np.random.rand(50)
y = np.random.rand(50)

plt.scatter(x, y)
plt.title("Диаграмма рассеяния")
plt.xlabel("X")
plt.ylabel("Y")
plt.show()
```

### 3. Сохранение графика

```python
plt.plot(x, y)
plt.title("График для сохранения")
plt.xlabel("Ось X")
plt.ylabel("Ось Y")
plt.savefig("plot.png")  # Сохраняет график в файл
```

## Основные компоненты Seaborn

Seaborn строится на Matplotlib, но включает в себя более высокоуровневые функции для создания более сложных и красивых графиков с минимальными усилиями.

### 1. Тема оформления

Seaborn предлагает несколько встроенных тем для стилизации графиков.

```python
sns.set_theme()  # Устанавливает стандартную тему оформления
```

### 2. Построение гистограммы с Seaborn

```python
sns.histplot(data, kde=True, bins=30)
plt.title("Гистограмма с KDE")
plt.show()
```

- **`kde=True`** — добавляет линию плотности вероятности.

### 3. Диаграмма рассеяния с Seaborn

```python
sns.scatterplot(x=x, y=y)
plt.title("Диаграмма рассеяния с Seaborn")
plt.show()
```

### 4. Парный график (Pairplot)

```python
# Создание парного графика для всех переменных в DataFrame
import seaborn as sns
import pandas as pd

# Пример данных
iris = sns.load_dataset("iris")
sns.pairplot(iris, hue="species")
plt.show()
```

### 5. Тепловая карта (Heatmap)

```python
import numpy as np

# Генерация случайной матрицы данных
data = np.random.rand(10, 12)
sns.heatmap(data, annot=True, cmap="coolwarm")
plt.title("Тепловая карта")
plt.show()
```

### 6. Корреляционная матрица

```python
# Вычисление корреляции между столбцами
corr = iris.corr()
sns.heatmap(corr, annot=True, cmap="coolwarm")
plt.title("Корреляционная матрица")
plt.show()
```

### 7. Boxplot (Ящичная диаграмма)

```python
sns.boxplot(x="species", y="sepal_length", data=iris)
plt.title("Ящичная диаграмма")
plt.show()
```

## Расширенные примеры

### 1. Столбчатая диаграмма

```python
import seaborn as sns

# Пример данных
tips = sns.load_dataset("tips")
sns.barplot(x="day", y="total_bill", data=tips)
plt.title("Средний счет по дням недели")
plt.show()
```

### 2. Линейный график с ошибками

```python
# Создание линейного графика с отображением ошибок
sns.lineplot(x="size", y="total_bill", data=tips, ci="sd")
plt.title("Линейный график с ошибками")
plt.show()
```

### 3. Настройка подписей и размеров

```python
plt.figure(figsize=(10, 6))  # Установка размера графика
sns.lineplot(x="size", y="total_bill", data=tips)
plt.title("Линейный график", fontsize=16)
plt.xlabel("Размер группы", fontsize=12)
plt.ylabel("Общий счет", fontsize=12)
plt.show()
```

## Дополнительные возможности

### 1. Многослойные графики

```python
sns.lineplot(x="size", y="total_bill", data=tips, label="Группы")
sns.scatterplot(x="size", y="total_bill", data=tips, color="red", label="Точки")
plt.title("Многослойный график")
plt.legend()
plt.show()
```

### 2. Регрессионный график

```python
sns.regplot(x="size", y="total_bill", data=tips, scatter_kws={'s':10}, line_kws={'color':'red'})
plt.title("Регрессионный график")
plt.show()
```

### 3. Создание сублинейных графиков

```python
sns.FacetGrid(tips, col="sex", row="time").map(sns.scatterplot, "size", "total_bill")
plt.show()
```

## Полезные ссылки

- [Документация Matplotlib](https://matplotlib.org/stable/contents.html)
- [Документация Seaborn](https://seaborn.pydata.org/)
- [Пример с seaborn gallery](https://seaborn.pydata.org/examples/index.html)