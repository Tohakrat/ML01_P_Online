# Homework 04
<hr>


## Задание
Имеется файл `*.csv` со следующими полями: `UserId`, `movieId`, `rating`, `timestamp`.
Необходимо найти:
1. Общее число оценок в файле
2. Общее количество пользователей, поставивших оценки
3. Общее количество оцененных фильмов
4. ID самого активного пользователя
5. Фильм, собравший наибольшее количество оценок
<hr>


## Структура репозитория
Структура проекта `HW_04` имеет следующий вид:
+ в каталоге **`data`** содержатся файлы **`.csv`** (и/или **`.txt`**) с исходными данными для обработки;
+ в каталоге **`src`** содержатся файлы **`.py`** с исходным кодом:
    * в файле **`hw_04.py`** находится код основной программы;
    * в файле **`hw_04_spacy.ipynd`** находится код основной программы для `Jupiter Notebook`.
+ в файле **`requirements.txt`** содержится список подключаемых библиотек.


## ВАЖНОЕ ПРИМЕЧАНИЕ!
Так как в файле `.gitignore` репозитория `ML01_P_Online` прописана строка, 
которая исключает `.csv` файлы из области видимости `git`, то исходные данные продублированы в `.txt` файл. 
Программный код по умолчанию загружает данные из `.csv` файла.
Если он отсутствует - данные берутся с `.txt` файла.
<hr>


## Анализ ТЗ
* В качестве инструмента для анализа и обработки табличных данных (в данном случае содержимое `.csv` файла) 
предлагается использовать библиотеку `Pandas`. Данная библиотека предоставляет готовый набор инструментов, 
позволяющий быстро и удобно осуществлять обработку табличных данных.
* Данные в `Pandas` хранятся в виде следующих структур: 
  - `Series` - одномерный массив;
  - `DataFrame` - двумерная таблица (массив).
* В нашем случае для последующей обработки данных удобно использовать формат `DataFrame`.
* После изучения доступных методов и функций библиотеки `Pandas` для решения поставленной задачи воспользуемся следующими:
  - для чтения `.csv` файлов используем метод `pandas.read_csv('file.csv')` (например, `df = pandas.read_csv('file.csv')`);
  - для удаления строк с пустыми значениями используем метод `df.dropna()`;
  - для выбора конкретного столбца используем `df['column']`;
  - для фильтрации данных используем `filtered_data = df[df['column_name'] > value]`;
  - для подсчёта количества уникальных значений в столбце используем `df['column'].value_counts()`;
  - для подсчёта максимального количества появлений любого уникального значения в столбце используем `df['column'].value_counts().max()`;
  - строка `df['column'].value_counts().idxmax()` вернет значение `column`, которое встречается в `df` наибольшее количество раз.


## Выполнение программы
Для запуска программы необходимо выполнить:
```
python.exe -m pip install -r .\requirements.txt
python.exe .\hw_04.py
```
<hr>


## Описание работы программы
Ниже приведен листинг скрипта **`hw_04.py`**:
```
import os
import pandas as pd


"""
ЗАДАНИЕ. 
Имеется файл .csv со следующими полями: 'UserId', 'movieId', 'rating', 'timestamp'.
Необходимо найти:
1. Общее число оценок в файле
2. Общее количество пользователей, поставивших оценки
3. Общее количество оцененных фильмов
4. ID самого активного пользователя
5. Фильм, собравший наибольшее количество оценок
"""


# Путь к файлу с данными
file_path = "../data/ratings_small.csv"
file_path = file_path if os.path.isfile(file_path) else "../data/ratings_small.txt"


if __name__ == "__main__":

    # 1) Загружаем данные из файла и отбрасываем строки с пропущенными значениями
    df = pd.read_csv(file_path)
    print(f"Файл '{file_path}' успешно импортирован.")
    df.dropna()

    # 2) Общее число оценок в файле
    print(f"Общее число оценок в файле: {len(df[df['rating'] > 0])}.")

    # 3) Общее число пользователей, поставивших оценки
    print(f"Общее число пользователей, поставивших оценки: {len(df['userId'].value_counts())}.")

    # 4) Общее количество оцененных фильмов
    print(f"Общее количество оцененных фильмов: {len(df['movieId'].value_counts())}.")

    # 5) ID самого активного пользователя:
    print(f"ID самого активного пользователя: UserId={df['userId'].value_counts().idxmax()} ({df['userId'].value_counts().max()} оценка/и/ок).")

    # 6) Фильм, собравший наибольшее количество оценок:
    print(f"Фильм, собравший наибольшее количество оценок: movieId={df['movieId'].value_counts().idxmax()} ({df['movieId'].value_counts().max()} оценка/и/ок)).")
```
_В командной строке получим следующее:_
```
Файл '../data/ratings_small.csv' успешно импортирован.
Общее число оценок в файле: 100004.
Общее число пользователей, поставивших оценки: 671.
Общее количество оцененных фильмов: 9066.
ID самого активного пользователя: UserId=547 (2391 оценка/и/ок).
Фильм, собравший наибольшее количество оценок: movieId=356 (341 оценка/и/ок)).
```