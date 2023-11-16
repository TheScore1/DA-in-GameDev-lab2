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
- Сделать экономическую модель выбранного ресурса. В моём случае металлолом из игры Rust. Rust — многопользовательская компьютерная игра в жанре симулятора выживания, разрабатываемая независимой британской студией Facepunch, ранее создавшей Garry's Mod. Вам предстоит собирать ресурсы, а также убивать животных и других игроков ради заветного лута (или просто так).
- Выбранный мною ресурс - металлолом. Он один из самых основных ресурсов, нужный везде. Используется для крафтов, торговли и изучения новых рецептов на верстаках. Подробнее изображено ниже:
![image](https://github.com/TheScore1/DA-in-GameDev-lab2/assets/113776816/256da3b2-3498-4b1e-afcb-d31e6cfaeb18)

![Без имени](https://github.com/TheScore1/DA-in-GameDev-lab2/assets/113776816/d42e4f8a-db54-4a5f-8b1a-d8a807cfc583)

## Задание 2
### Заполнить гугл таблицу через питон.
Сначала нужно включить google drive api для проекта, а затем включить google sheets api, после чего я сохранил файл с ключом на мой компьютер. После чего создал таблицу и разрешил редактировать её созданному сервисному аккаунту. После написал скрипт для заполнения таблицы данными из .ipynb файла. Результат выполнения скрипта и сам скрипт можно увидеть ниже

- Результат
![image](https://github.com/TheScore1/DA-in-GameDev-lab2/assets/113776816/635bde6a-b280-426a-acdf-696d0283e1b8)
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
Для начала я добавил в проект jsonPackage и soundPackage для работы с таблицей и воспроизведения звуков.  После чего создал пустой объект и подключил к нему скрипт, в котором написал код
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodFarm;
    public AudioClip normalFarm;
    public AudioClip badFarm;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;

    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet.Count == 0) return;

        if (dataSet["Mon_" + i.ToString()] <= 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 100 & dataSet["Mon_" + i.ToString()] < 700 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] >= 700 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1yIn6akUuvAWgalFM-_lc4opTADlwWsiqjB7d1oRKyFk/values/Лист1?key=AIzaSyDJQupEMQizySUeP1W0slQqqwP0mliTKy8");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodFarm;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalFarm;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badFarm;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```
- Звуки воспроизводятся корректно, в соответствии со значениями из таблицы. Ниже вывод консоли:
![image](https://github.com/TheScore1/DA-in-GameDev-lab2/assets/113776816/d9dadae8-1ad3-4a5e-ae6f-92eeaa762f36)

- Результат можно скачать и прослушать в юнити, чтобы убедиться в рабостоспособности проекта.
  
## Выводы

В этой лабораторной работе я научился подключать api для работы с таблицами, взаимодействовать с гугл таблицами через питон, а так же использовать информацию из таблицы в юнити.

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
