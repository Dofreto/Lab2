# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Гребёнкин Данил Антонович
- ФО-230005

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | ? |
| Задание 2 | * | ? |
| Задание 3 | * | ? |

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
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Ответ на вопрос.
- Выводы.
- ✨Magic ✨

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1
### Выбрать одну из игровых переменных в игре "СПАСТИ РТФ: Выживание" и проанализировать её.
Ход работы:
- Выбрать переменную.
- Описать её роль в игре, условия изменения / появления и диапазон допустимых значений.
- Построить схему экономической модели в игре.
- Указать место выбранного ресурса в схеме.


#### Выбранная переменная: 
Монеты

#### Роль в игре: 
Монеты приобретаются для возобновления боеприпасов, покупки оружия, медикаментов и для прокачки игровых навыков. Выпадают с убитых зомби, после просмотра рекламы и из сундуков

#### Схема:
![Снимок экрана 2024-10-07 200405](https://github.com/user-attachments/assets/e222dc7f-3f57-46a7-867c-7a5c1e976729)

#### Его место: 
Предмет занимает ключевую роль в игре. Без монет не будет функционировать вся экономика игры.


## Задание 2
### С помощью скрипта на языке Python заполнить и визуализировать google-таблицу.
Ход работы:
- Написать скрипт на  Python.
- Заполнить google-таблицу данными, описывающие монеты.
- Визуализировать данные в google-таблице.
#### Условия написания скрипта 
Количество монет меняется в зависимости от действий игрока и его навыков. Поскольку есть множество вариантов изменения этого параметра, я, для простоты, добавил некоторые условности:
- Игрок поднимает монеты со всех убитых зомби
-  Игрок играет в всего 5 раундов.
-  После каждых двух раундов игрок может потратить деньги в магазине на патроны (может потратить от 5 до 25, или кратные 5, но до 25).
-  Игрок не использует функцию просмотра рекламы и открытие сундуков.

#### Скрипт на языке Python:
Написать код на подобие кода ниже:
```py

import gspread
import numpy as np

# Подключение к Google Sheets
gc = gspread.service_account(filename='unitydatascience2-437613-ae095d93eed1.json')
sh = gc.open('UnityServiceDataScince2')
worksheet = sh.sheet1

sh.sheet1.clear() # Очистка таблицы от старых данных

# Начальные параметры
total_rounds = 5
zombies_per_round = 10
current_coins = 0

# Обновление таблицы
row = 1
sh.sheet1.update_acell('A' + str(row), 'Раунд')
sh.sheet1.update_acell('B' + str(row), 'Убито зомби')
sh.sheet1.update_acell('C' + str(row), 'Монеты после раунда')
sh.sheet1.update_acell('D' + str(row), 'Потраченные монеты')

for round_number in range(1, total_rounds + 1):
    # Убийство зомби
    zombies_killed = zombies_per_round
    current_coins += zombies_killed

    # Запись данных о раунде в таблицу
    row += 1
    sh.sheet1.update_acell('A' + str(row), str(round_number))
    sh.sheet1.update_acell('B' + str(row), str(zombies_killed))
    sh.sheet1.update_acell('C' + str(row), str(current_coins))

    # Проверка, нужно ли покупать патроны
    if round_number % 2 == 0:
        # Предположим, игрок может потратить от 5 до 25 монет, кратные 5
        spent_coins = min(current_coins, (current_coins // 5) * 5)  # Максимум кратное 5
        if spent_coins > 25:
            spent_coins = 25

        current_coins -= spent_coins
        sh.sheet1.update_acell('D' + str(row), str(spent_coins))
    else:
        sh.sheet1.update_acell('D' + str(row), '0')

    # Увеличение количества зомби для следующего раунда
    zombies_per_round += 10

print("Данные успешно записаны в Google Таблицу.")

```

#### Визуализация данных в google-таблице:
![Снимок экрана 2024-10-12 222342](https://github.com/user-attachments/assets/ee366e59-cab1-4d25-a764-68455e6a590d)

Ссылка на google-таблицу: https://docs.google.com/spreadsheets/d/1etzL3dBC1fllgRFkuOMtoiuI78qffmFtAwma5M3TQ-8/edit?gid=0#gid=0


## Задание 3
### Воспроизведение звуковых файлов на сцене Unity
Настроить на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной.
Ход работы:
- Настроить на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной
- Написать скрипт на языке С#

Вот так бы всегда
неплохо(Сидор)
Ты бы ещё консервных банок насобирал


Работа над игрой про зомби. Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения монет. Условия получения монет:
- +1 монета за каждого убитого зомби. - +1 монета за попадание в конечности зомби (невсегда можно получить). - увеличение количества получаемых монет взависимоти от вкаченного навыка на повышение получения монет (увеличение от 1 до 10).
При достижении игроком суммы 1000 монет проигрывается первый аудиофайл. При достижении игроком суммы 3000 монет проигрывается второй аудиофайл. При достижении игроком суммы 5000 монет проигрывается третий аудиофайл

При написании кода и скрипат нужно отталкиваться от данных примеров: 
На Python (Python взаимодейтсвует с google-таблицей. В google-таблице отображаются данные с Python):
```py
import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascience2-437613-ae095d93eed1.json')
sh = gc.open('UnityServiceDataScince2')
price = np.random.randint(2000,10000,11)
mon = list(range(1,11))
i=0
while i <= len(mon):
    i+=1
    if i==0:
        continue
    else:
        tempInf=((price[i-1]-price[i-2])/price[i-2])*100
        tempInf=str(tempInf)
        tempInf=tempInf.replace('.',',')
        sh.sheet1.update(('A'+str(i)),str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)
```

Работа над игрой про зомби. Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения монет. Условия получения монет:
+1 монета за каждого убитого зомби. - +1 монета за попадание в конечности зомби (не всегда можно получить). - увеличение количества получаемых монет в зависимости от вкаченного навыка на повышение получения монет (увеличение от 1 до 10). - +100 монет за просмотр рекламы (можно смотреть не ограниченно число раз, но игроку не хочется тратить время на рекламу, поэтому он смотрит её не часто). - + 50 монет за открытие сундуков.
При достижении игроком суммы 1000 монет проигрывается первый аудиофайл. При достижении игроком суммы 3000 монет проигрывается второй аудиофайл. При достижении игроком суммы 5000 монет проигрывается третий аудиофайл
При написании кода и скрипта нужно отталкиваться от данных примеров:
На Python (Python взаимодействует с google-таблицей. В google-таблице отображаются данные с Python):

```py

import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascience2-437613-ae095d93eed1.json')
sh = gc.open('UnityServiceDataScince2')
price = np.random.randint(2000,10000,11)
mon = list(range(1,11))
i=0
while i <= len(mon):
    i+=1
    if i==0:
        continue
    else:
        tempInf=((price[i-1]-price[i-2])/price[i-2])*100
        tempInf=str(tempInf)
        tempInf=tempInf.replace('.',',')
        sh.sheet1.update(('A'+str(i)),str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)

```

На C# (так же взаимодействеут с google-таблицей):
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;
public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
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
        if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }
    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1etzL3dBC1fllgRFkuOMtoiuI78qffmFtAwma5M3TQ-8/values/Лист1?key=AIzaSyD9xlsZ-yk-X-kdJ489GSt_0SQWjl_gcvc");
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
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}


## Выводы

Абзац умных слов о том, что было сделано и что было узнано.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by





Работа над игрой про зомби. Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения монет. Условия получения монет:
+1 монета за каждого убитого зомби. - +1 монета за попадание в конечности зомби (не всегда можно получить). - увеличение количества получаемых монет в зависимости от вкаченного навыка на повышение получения монет (увеличение от 1 до 10). - +100 монет за просмотр рекламы (можно смотреть не ограниченно число раз, но игроку не хочется тратить время на рекламу, поэтому он смотрит её не часто). - + 50 монет за открытие сундуков.
При достижении игроком суммы 1000 монет проигрывается первый аудиофайл. При достижении игроком суммы 3000 монет проигрывается второй аудиофайл. При достижении игроком суммы 5000 монет проигрывается третий аудиофайл.
Уже написан код на Python, нужно написать скрипт для аудио файлов на C#, который будет взаимодействовать с таблицей. Код на Python:
```py

import gspread
import numpy as np

# Подключение к Google Sheets
gc = gspread.service_account(filename='unitydatascience2-437613-ae095d93eed1.json')
sh = gc.open('UnityServiceDataScince2')
worksheet = sh.sheet1

sh.sheet1.clear() # Очистка таблицы от старых данных

# Настройка симуляции игры
num_sessions = 10  # количество игровых сессий
zombies_killed = np.random.randint(1, 10, num_sessions)  # количество убитых зомби
limb_hits = np.random.randint(0, 5, num_sessions)  # количество попаданий в конечности
ads_watched = np.random.choice([0, 1], num_sessions, p=[0.8, 0.2])  # просмотр рекламы (редко)

skill_multiplier = np.random.randint(1, 11)  # уровень умения (1-10)
chests_opened = np.random.randint(0, 3, num_sessions)  # количество открытых сундуков

# Расчет набора монет
coin_totals = []
current_coins = 0

for i in range(num_sessions):
    coins_from_zombies = int(zombies_killed[i]) * skill_multiplier
    coins_from_limbs = int(limb_hits[i]) * skill_multiplier
    coins_from_ads = int(ads_watched[i]) * 100
    coins_from_chests = int(chests_opened[i]) * 50

    # Обновление монет
    current_coins += coins_from_zombies + coins_from_limbs + coins_from_ads + coins_from_chests
    coin_totals.append(current_coins)

    # Обновление Google Sheet
    worksheet.update(f'A{i + 1}', f'Session {i + 1}')
    worksheet.update(f'B{i + 1}', int(current_coins))  # Преобразование в обычный int

    milestone_message = ""
    if current_coins >= 5000:
        milestone_message = "Audio 3 Milestone"
    elif current_coins >= 3000:
        milestone_message = "Audio 2 Milestone"
    elif current_coins >= 1000:
        milestone_message = "Audio 1 Milestone"
    else:
        milestone_message = "Progressing"

    worksheet.update(f'C{i + 1}', milestone_message)

```
Есть файл (Google-таблица): https://sheets.googleapis.com/v4/spreadsheets/1etzL3dBC1fllgRFkuOMtoiuI78qffmFtAwma5M3TQ-8/values/Лист1?key=AIzaSyD9xlsZ-yk-X-kdJ489GSt_0SQWjl_gcvc.
Нужно написать скрипт, который будет воспроизводить первый аудиофайл при значении второй строки равном 1000; второй аудиофайл при значении второй строки равном 3000; третий аудиофайл при значении второй строки равном 5000
