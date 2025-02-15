```markdown
# Cheatsheet по Pandas

**Pandas** — это мощная библиотека Python для обработки и анализа данных. Она предоставляет структуры данных и инструменты для работы с таблицами и временными рядами, включая DataFrame (для работы с таблицами) и Series (для работы с одномерными массивами).

## Установка

```bash
pip install pandas
```

## Основные компоненты Pandas

### 1. Импорт библиотеки

```python
import pandas as pd
```

### 2. Создание объектов

- **Series** — одномерный массив с метками.

```python
# Создание Series из списка
data = [10, 20, 30, 40, 50]
series = pd.Series(data)
print(series)
```

- **DataFrame** — двумерная таблица данных (аналог таблицы Excel или SQL-таблицы).

```python
# Создание DataFrame из словаря
data = {'Name': ['Alice', 'Bob', 'Charlie'],
        'Age': [25, 30, 35],
        'City': ['New York', 'Los Angeles', 'Chicago']}
df = pd.DataFrame(data)
print(df)
```

### 3. Загрузка данных

- **Из CSV файла**

```python
df = pd.read_csv('file.csv')
```

- **Из Excel файла**

```python
df = pd.read_excel('file.xlsx')
```

- **Из SQL базы данных**

```python
import sqlite3
conn = sqlite3.connect('database.db')
df = pd.read_sql_query("SELECT * FROM table_name", conn)
```

### 4. Основные методы для работы с данными

- **Просмотр первых и последних строк DataFrame**

```python
print(df.head())  # Печатает первые 5 строк
print(df.tail())  # Печатает последние 5 строк
```

- **Проверка информации о DataFrame**

```python
print(df.info())  # Информация о типах данных и количестве значений
print(df.describe())  # Описание статистики для числовых данных
```

- **Доступ к столбцам и строкам**

```python
# Доступ к столбцам
print(df['Age'])

# Доступ к строкам по индексу
print(df.iloc[1])  # Строка с индексом 1
```

- **Фильтрация данных**

```python
# Фильтрация по условию
filtered_df = df[df['Age'] > 30]
print(filtered_df)
```

- **Доступ к нескольким столбцам**

```python
# Получение нескольких столбцов
subset_df = df[['Name', 'Age']]
print(subset_df)
```

### 5. Модификация данных

- **Изменение значений в столбце**

```python
df['Age'] = df['Age'] + 1  # Увеличиваем возраст на 1
```

- **Добавление нового столбца**

```python
df['Country'] = ['USA', 'USA', 'USA']
```

- **Удаление столбца**

```python
df.drop('Country', axis=1, inplace=True)  # Удаление столбца по имени
```

- **Изменение индекса**

```python
df.set_index('Name', inplace=True)  # Устанавливаем столбец 'Name' как индекс
```

### 6. Обработка пропущенных данных

- **Проверка пропущенных значений**

```python
print(df.isnull().sum())  # Количество пропущенных значений в каждом столбце
```

- **Заполнение пропущенных значений**

```python
df['Age'].fillna(df['Age'].mean(), inplace=True)  # Заполнение пропущенных значений средним
```

- **Удаление строк с пропущенными значениями**

```python
df.dropna(inplace=True)  # Удаление строк с пропущенными значениями
```

### 7. Группировка и агрегация данных

- **Группировка данных по столбцу и вычисление статистики**

```python
grouped = df.groupby('City')['Age'].mean()  # Средний возраст по городу
print(grouped)
```

- **Применение нескольких агрегаций**

```python
aggregated = df.groupby('City').agg({'Age': ['mean', 'sum'], 'Name': 'count'})
print(aggregated)
```

### 8. Слияние и объединение данных

- **Объединение двух DataFrame по общему столбцу (merge)**

```python
df1 = pd.DataFrame({'ID': [1, 2, 3], 'Value': ['A', 'B', 'C']})
df2 = pd.DataFrame({'ID': [1, 2, 4], 'Description': ['Desc1', 'Desc2', 'Desc4']})
merged_df = pd.merge(df1, df2, on='ID', how='inner')  # Внутреннее соединение по ID
print(merged_df)
```

- **Объединение (concat) данных по строкам или столбцам**

```python
df3 = pd.DataFrame({'ID': [4, 5], 'Value': ['D', 'E']})
concatenated_df = pd.concat([df1, df3], ignore_index=True)
print(concatenated_df)
```

### 9. Сохранение данных

- **Сохранение в CSV файл**

```python
df.to_csv('output.csv', index=False)
```

- **Сохранение в Excel файл**

```python
df.to_excel('output.xlsx', index=False)
```

### 10. Визуализация данных с помощью Pandas и Matplotlib

- **Графики с использованием Pandas**

```python
df['Age'].plot(kind='bar')  # Построение столбчатой диаграммы
plt.show()
```

- **Гистограмма**

```python
df['Age'].plot(kind='hist', bins=10)
plt.show()
```

- **Точечная диаграмма**

```python
df.plot(kind='scatter', x='Age', y='City')
plt.show()
```

## Полезные ссылки

- [Документация Pandas](https://pandas.pydata.org/pandas-docs/stable/)
- [Руководство по Pandas для начинающих](https://pandas.pydata.org/pandas-docs/stable/getting_started/intro_tutorials/index.html)
