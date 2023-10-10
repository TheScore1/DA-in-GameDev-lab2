# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Ежов Никита Олегович
- РИ220950
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Составить модель какого-либо ресурса и т.д.
- Задание 2.
- Научиться взаимодействовать с гугл таблицами через питон.
- Задание 3.
- В соответствии с данными с таблицы воспроизводить звуки в юнити.
- Выводы.
- ✨Magic ✨

## Цель работы
Составлять экономическую модель ресурса. Взаимодействовать с таблицами и взаимодействие юнити с ними.

## Задание 1
### Сделать экономическую модель.
- Сделать экономическую модель выбранного ресурса. В моём случае металлолом из игры Rust.
![Без имени](https://github.com/TheScore1/DA-in-GameDev-lab2/assets/113776816/d42e4f8a-db54-4a5f-8b1a-d8a807cfc583)

## Задание 2
### Заполнить гугл таблицу через питон.

- Результат
![image](https://github.com/TheScore1/DA-in-GameDev-lab2/assets/113776816/397f92cd-2ded-474a-bcc0-f20bb09acd01)

- Код

```py

import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascience-400216-d24bc9bcfacd.json')
sh = gc.open("UnityDataScience")
price = np.random.randint(2000, 100000, 11)
mon = list(range(1,11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        tempInf = int(abs(((price[i-1]-price[i-2])/price[i-2])*5*100))
        tempInf = str(tempInf)
        tempInf = tempInf.replace('.',',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)

```

## Задание 3
### Воспроизвести звуки в юнити в соответствии с значениями.

- Можно скачать проект и прослушать в юнити.
  
## Выводы

В этой лабораторной работе я научился взаимодействовать с гугл таблицами и юнити.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
