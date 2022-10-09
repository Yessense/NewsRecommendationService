# NewsRecommendationService

*Хакатон ВТБ трек DATA*

*Команда "Герои ML и магии"*

## Краткое описание сервиса

Парсер мониторит некоторое количество новостных каналов.

Получив новость, мы извлекаем ее эмбеддинг, возможно предварительно суммаризировав её.

Когда нам приходит запрос, который содержит роль (должность) сотрудника и временной промежуток, то мы выбираем и
возвращаем 2-3 наиболее подходящие новости за этот временной промежуток, сопоставляя эмбеддинг описания должности и
эмбеддинги новостей.

## Инструкция

### Загрузка новостей в базу
- Поднять API к базе данных из репозитория `https://github.com/Leon200211/api_vtb`
- Собрать Docker контейнер и запустить `build.sh`, `run.sh`
- Положить описание ролей в папку `./roles_description/` (один текстовый файл для одной роли)
- Выделить векторные представления из описания ролей `roles.py`
- Запустить файл `website_parser.py`
- Поднять Телеграм бота из репозитория `https://github.com/Leon200211/api_vtb`
- Написать запрос телеграм боту

<img src="images/photo_2022-10-09_10-49-25.jpg" alt="drawing" width="800"/>

#### Docker контейнер

##### Определение похожести векторов

```python

import requests

json = {
    'target': [1., 2, 3, 4, 5],
    'source': [[1, 2, 3., 4, 5],
               [2, 3, 4., 5, 6],
               [3, 4, 5., 6, 7]]
}
similarities = requests.post("http://127.0.0.1:8080/get_similarity", json=json)
print(similarities.text)

>>> [0.9999998807907104, 0.9949367046356201, 0.9864400625228882]
```

##### Получение векторных пресдтавлений

```python
import requests

json = {
    'news': ['Приятная новость номер один',
             'Эмбеддер запущен в докер контейнере',
             'Api через Flask']
}

embeddings = requests.post("http://127.0.0.1:8080/get_embeddings", json=json)
print(embeddings.text)

>>> [[0.43907254934310913, -0.8340410590171814, 0.3247581720352173, 
      -0.4997189939022064, -0.09722471982240677, -0.4234312176704407, 
      1.0250037908554077, -1.0949842929840088 ...]]

```

## Инструкция по установке/использованию



...

Тренды выявляются путём кластеризация краткого описания статей - задача, которую мы производим на облаке, чтобы не нагружать основной продукт зависимостями от кучи пакетов и не использовать своё ОЗУ. Результаты мы сохраняем в json, а после рецензии администратора в базу данных. Резлуьтаты представляют из себя 10 самых ключевых слов каждого из кластеров статей, то есть слова определяющие тематику кластера. По ним можно поределяить какие тренды и инсайты можно вывести.

Код: https://www.kaggle.com/code/annkar/clustervtb

## Пример использования

...

Определение трендов
Надо запустить ноутбук, предварительно, загрузив туда данные в формате csv. В кластеризации (dbscan_search) можно указать eps_grid - какие минимальные косинусные растояния попробовать использовать и min_samples_grid - какие минимальные размеры кластеров попробовать использовать.

## Картинки

### Визуализация кластеризации
![image](https://user-images.githubusercontent.com/82397895/194742704-86c766a5-6c99-44be-96f3-2401b3a339bd.png)
Файлы результатов: ![image](https://user-images.githubusercontent.com/82397895/194742907-f4e13e7a-63cb-4b96-9a91-ce5072e08806.png)

### Схема пайплайна
![image](https://user-images.githubusercontent.com/39330120/194748858-9fe6f415-ccb2-481f-a78c-6e5410045ffd.png)

### Схема архетиктуры сервиса

![image](https://user-images.githubusercontent.com/39330120/194748829-f23a6fe0-a7da-4ffb-a4c4-2af55a4bdb56.png)

