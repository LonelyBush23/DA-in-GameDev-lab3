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

```python
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if(distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```

- Объекту «сфера» добавить компоненты Rigidbody, Decision Requester, Behavior Parameters и настройть их.
<img width="217" alt="image" src="https://user-images.githubusercontent.com/104368430/198010979-850a2702-3854-490e-b382-fd6863b3ff61.png">

- В корень проекта добавьте файл конфигурации нейронной сети. Запустить работу ml-агента
<img width="610" alt="image" src="https://user-images.githubusercontent.com/104368430/198011654-eeffe77f-92de-48e4-9b37-fe83f011ace0.png">

```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```

- Сделайте 3, 9, 27 копий модели «Плоскость-Сфера-Куб», запустите симуляцию сцены и наблюдайте за результатом обучения модели
<img width="549" alt="image" src="https://user-images.githubusercontent.com/104368430/198016696-9bed706f-db49-4feb-9cd1-2914714f216b.png">

## Вывод
Чем больше копий модели «Плоскость-Сфера-Куб», тем более высокого показателя точности мы дочтигаем за меньшее время итераций.

## Задание 2
### Подробно опишите каждую строку файла конфигурации нейронной сети, доступного в папке с файлами проекта по ссылке. Самостоятельно найдите информацию о компонентах Decision Requester, Behavior Parameters, добавленных на сфере.
```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```

- trainer_type: ppo - тип обучения с поощрением.
- batch_size: Определяет количество информации, собираемой в ходе одного шага в градиентном спуске. Всегда должно быть в несколько раз меньше, чем buffer_size.
- buffer_size: Количество опыта, который необходимо собрать перед обновлением модели политики. Соответствует тому, сколько опыта должно быть собрано, прежде чем мы приступим к какому-либо изучению или обновлению модели. Это должно быть в несколько раз больше, чем batch_size. Обычно больший размер буфера соответствует более стабильным обновлениям обучения.
- learning_rate: Начальная скорость обучения для градиентного спуска. Соответствует силе каждого шага обновления градиентного спуска. Обычно это значение следует уменьшить, если тренировка нестабильна, а вознаграждение не увеличивается последовательно.
- beta: Сила регуляризации энтропии, которая делает политику "более случайной". Это гарантирует, что агенты должным образом исследуют пространство действий во время обучения. Увеличение этого значения обеспечит выполнение большего количества случайных действий. Это должно быть скорректировано таким образом, чтобы энтропия медленно уменьшалась вместе с увеличением вознаграждения. Если энтропия падает слишком быстро, нужно увеличить бета-версию. Если энтропия падает слишком медленно, уменьшить бета.
- epsilon: Влияет на то, насколько быстро политика может развиваться во время обучения. Соответствует допустимому порогу расхождения между старой и новой политиками при обновлении с градиентным спуском. Установка этого значения небольшим приведет к более стабильным обновлениям, но также замедлит процесс обучения.
- lambd: Параметр регуляризации (лямбда), используемый при расчете Обобщенной оценки преимущества (GAE). Это можно рассматривать как то, насколько агент полагается на свою текущую оценку стоимости при расчете обновленной оценки стоимости. Низкие значения соответствуют большей зависимости от текущей оценки ценности (что может быть большой погрешностью), а высокие значения соответствуют большей зависимости от фактических вознаграждений, полученных в окружающей среде (что может быть высокой дисперсией). Параметр обеспечивает компромисс между ними, и правильное значение может привести к более стабильному процессу обучения.
- num_epoch: Количество проходов, которые необходимо выполнить через буфер опыта при выполнении оптимизации градиентного спуска.Чем больше размер пакета, тем больше это допустимо сделать. Уменьшение этого параметра обеспечит более стабильные обновления за счет более медленного обучения.
- learning_rate_schedule : Определяет, как скорость обучения меняется с течением времени.
- normalize: Применяется ли нормализация к входным данным векторного наблюдения. Эта нормализация основана на текущем среднем значении и дисперсии векторного наблюдения. Нормализация может быть полезна в случаях со сложными задачами непрерывного управления, но может быть вредна при более простых задачах дискретного управления.
- hidden_units: Соответствует количеству единиц в каждом полностью подключенном слое нейронной сети. Для простых задач, где правильным действием является простая комбинация входных данных наблюдения, это должно быть небольшим. Для задач, где действие представляет собой очень сложное взаимодействие между переменными наблюдения, это должно быть больше.
- num_layers: Определяет количество слоев нейронной сети. Для простых задач меньшее количество слоев, скорее всего, будет обучаться быстрее и эффективнее. Для более сложных задач управления может потребоваться больше уровней.
gamma: Коэффициент дисконтирования для будущих вознаграждений, поступающих от окружающей среды. Это можно рассматривать как то, насколько далеко в будущем агент должен заботиться о возможных вознаграждениях. В ситуациях, когда агент должен действовать в настоящем, чтобы подготовиться к вознаграждению в отдаленном будущем, это значение должно быть большим. В тех случаях, когда вознаграждение более немедленное, оно может быть меньше. Должно быть строго меньше 1.
-strength: Фактор, на который можно умножить вознаграждение, получаемое от окружающей среды. Типичные диапазоны будут варьироваться в зависимости от сигнала вознаграждения.
- max_steps: Максимальное количество проходов.
- time_horizon: Временной отрезок.
- summary_freq: Суммированная частота.
- DecisionRequester: Запрос на принятие решения вызывает CollectObservation, а затем получает последнее действие в OnActionReceived, основанное на этом новом собранном наблюдении. С действиями из TakeActionBetweenDecisions он только снова вызовет OnActionReceived без сбора новых наблюдений и выведет последнее действие, которое он получил от NN.
- BehaviorParameters: Определяет, сколько наблюдений мы принимаем и какую форму будут принимать выводимые действия.


## Задание 3
### Доработайте сцену и обучите ML-Agent таким образом, чтобы шар перемещался между двумя кубами разного цвета. Кубы должны, как и в первом задании, случайно изменять координаты на плоскости. 
Ход работы: 
- Добавить новый кубик, который (Target2)
<img width="616" alt="image" src="https://user-images.githubusercontent.com/104368430/205315014-fc0cea89-0167-4216-9f0d-5ec4a9f7afef.png">

- Для перемещения шара между двумя кубами разного цвета написать необходимый код:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public Transform Target1;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8 - 4, 0.5f, Random.value * 8 - 4);
        Target1.localPosition = new Vector3(Random.value * 8 - 4, 0.5f, Random.value * 8 - 4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);
        float distanceToTarget1 = Vector3.Distance(this.transform.localPosition, Target1.localPosition);

        if (distanceToTarget < 1.42f || distanceToTarget1 < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```
- После обучения MLagentа шар перемещается между двумя кубами разного цвета, при этом Кубы должны, как и в первом задании, случайно изменют координаты на плоскости. 


## Выводы
В ходе работы я познакомилась с программными средствами для создания системы машинного обучения и ее интеграции в Unity. Создала ML-агента и тренировала нейросеть, задача которой заключалась в управлении шаром.
Игровой баланс - это субъективное «равновесие» между персонажами, командами, тактиками игры и другими игровыми объектами. Игровой баланс - одно из требований к «честности» правил. Но некоторые оспекты нуждаются в очень сложных настройках, здесь нам и нужно машинное обучение, которое оптимизирует игру на основе её восприятия конкретным геймером и помогает решить уйму задач, включая настройку игрового баланса и адаптивное управление сложностью в зависимости от уровня игрока.
