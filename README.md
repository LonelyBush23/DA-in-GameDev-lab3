# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил(а):
- Деньщик Дарья Дмитриевна
- РИ-210950
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
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity.
Ход работы:
- Создайть новый пустой 3D проект на Unity.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/104368430/197689181-cf925718-61af-40b5-8a3f-1bba67100431.png">

- Скачайть папку с ML агентом. В созданный проект добавьть ML Agent, выбрав Window - Package Manager - Add Package from disk. Последовательно добавьть .json – файлы:
o	ml-agents-release_19 / com,unity.ml-agents / package.json
o	ml-agents-release_19 / com,unity.ml-agents.extensions / package.json
![image](https://user-images.githubusercontent.com/104368430/197690845-467bf411-5f2f-4eee-8ea1-eb944a5b8bbb.png)

- Далее запускаем Anaconda Prompt для возможности запуска команд через консоль.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/104368430/197691208-f0e13ff6-2340-4323-ad7f-0dccee2252f6.png">

- Пишем серию команд для создания и активации нового ML-агента, а также для скачивания необходимых библиотек:
o	mlagents 0.28.0;
o	torch 1.7.1;
<img width="650" alt="image" src="https://user-images.githubusercontent.com/104368430/197692035-0ccad4dc-a8c3-446a-a01d-0aa3342ed380.png">
<img width="438" alt="image" src="https://user-images.githubusercontent.com/104368430/197692066-c4f8500b-a99a-430e-b36e-50e9d6518712.png">
<img width="413" alt="image" src="https://user-images.githubusercontent.com/104368430/197692641-7589be21-dfae-42fb-b080-9b7244cce65b.png">
<img width="948" alt="image" src="https://user-images.githubusercontent.com/104368430/197700344-16a3f763-3eb1-4252-b5da-23abfda61402.png">
<img width="663" alt="image" src="https://user-images.githubusercontent.com/104368430/197700707-afc9f1b1-5131-466d-9b94-0ec93d4647c8.png">

- Создайть на сцене плоскость, куб и сферу так. Создайть простой C# скрипт-файл и подключите его к сфере:
<img width="960" alt="image" src="https://user-images.githubusercontent.com/104368430/197702658-0e7b0a67-2992-4980-97e0-713dedcbc2b1.png">

- В скрипт-файл RollerAgent.cs добавьте код, опубликованный в материалах лабораторных работ – по ссылке.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/104368430/197703217-410343f5-1d69-4d2e-b455-4e8a72b730a8.png">


- Объекту «сфера» добавить компоненты Rigidbody, Decision Requester, Behavior Parameters и настройть их.
<img width="217" alt="image" src="https://user-images.githubusercontent.com/104368430/198010979-850a2702-3854-490e-b382-fd6863b3ff61.png">

- В корень проекта добавьте файл конфигурации нейронной сети. Запустить работу ml-агента
<img width="610" alt="image" src="https://user-images.githubusercontent.com/104368430/198011654-eeffe77f-92de-48e4-9b37-fe83f011ace0.png">

- 



## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1. 
Ход работы:
- C помощью Python выгружаем данные из Лаб1 в "Google Таблтцы"
<img width="960" alt="image" src="https://user-images.githubusercontent.com/104368430/194604632-6f46d90e-9ffb-4371-a8e1-864461d699c6.png">
<img width="960" alt="image" src="https://user-images.githubusercontent.com/104368430/194604727-985167d2-f9de-4875-b968-3b0c1da70563.png">

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2.
Ход работы:
- Из полученных данных из Лаб1 изменяем диапазон для "dataSet["Mon_" + i.ToString()]"
<img width="960" alt="image" src="https://user-images.githubusercontent.com/104368430/194604988-5881802a-8eef-4239-9491-f6872ff94c9d.png">
<img width="960" alt="image" src="https://user-images.githubusercontent.com/104368430/194605179-f6044521-2b1a-4f37-9fae-dadedc551aba.png">


## Выводы
В ходе работы я познакомилась с программными средствами для организции передачи данных между инструментами google, Python и Unity. Научилась реализовывать функционал, позволяющий генерировать стоимость товара (ресурса или игрового объекта) в виде набора данных. Также в этой лабораторной работе на движке Unity реализовала функционал, позволяющий воспроизводить аудио-файлы со звуковой информацией в зависимости от значений входного набора данных из таблицы. 
