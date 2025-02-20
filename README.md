## Тестовое задание на должность Junior Frontend разработчика в AdGo


###Запуск приложения
`cd .\app\`
`npm start`
Приложение работает при запущенном сервере

### Описание проделанной работы

Код причесан, все работает. Пока что не заметил как-то косяков, поэтому загружаю, но позже еще дополнительно протестирую.

### Описание задачи
Наша компания занимается рекламой. Мы собираем огромное множество статистических данных о показах, кликах и стоимости рекламных объявлений. Одним из основных инструментов нашей системы является статистика. Статистика это интерфейс для просмотра и анализа подобного рода данных. Мы предлагаем реализовать Вам упрощенную версию данного инструмента.

Статистика - набор событий о показах рекламы. Показ происходит в определенной среде: устройстве, операционной системе, браузере... Также пользователь, увидевший рекламу, может кликнуть по рекламному объявлению и посмотреть рекламу. За просмотр рекламодатель оплачивает определенную сумму денег.

Если посмотреть на показы в таблице, то это будет выглядеть следующим образом:

| ID      | Platform | Operating system | Browser | Click | Money | Datetime            |
| ------- | -------- | ---------------- | ------- | ----- | ----- | ------------------- |
| 1       | mobile   | android          | chrome  | true  | 0.004 | 2019-08-13 07:43:41 |
| 2       | mobile   | ios              | safari  | false | 0     | 2019-08-13 13:27:57 |
| 3       | desktop  | windows          | chrome  | false | 0     | 2019-08-14 03:51:13 |
| 4       | mobile   | android          | opera   | false | 0     | 2019-08-14 17:33:21 |
| 5       | desktop  | macos            | firefox | true  | 0.08  | 2019-08-15 09:00:05 |

>В [репозитории находится api-сервер](server), который умеет общаться по `HTTP`. Данное api предоставит вам данные которые необходимы для выполнения данного задания. [Подробное описание api-сервера](#api) в конце readme.

### Задание
Вам нужно реализовать интерфейс с выводом статистических данных в таблицу. Добавить возможность сгруппировать данные, применить фильтры, а также просмотр в режиме постраничной навигации.

![scheme](docs/scheme.png "Scheme")
<p align="center"><i>Схематичное представление задания.</i></p>

Что должно быть сделано:

- Таблица с данными;
- Постраничная навигация, например по 10 или 25 строк;
- Фильтр периода за который происходит выбор статистики;
- Фильтр по платформе, браузерам и операционным системам, о которых будет отображена информация;
- Любые ваши дополнения привествуются.

### FAQ

<strong>Куда и как отправить решение?</strong>

Сделайте `fork` данного репозитория. Реализуйте свое решение в директории [app](app). Когда будете готовы, создайте `pull request` в `master` ветку нашего репозитория. Заодно мы оценим ваше умение работать с `GIT`.

<strong>Какие библиотеки и инструменты можно использовать?</strong>

Мы не хотим накладывать на вас много ограничений. Можете использовать любые библиотеки, но нам важно посмотреть **ваш код** и **ваши решения**.

> Обязательное условие: выполнить проект на [React](https://reactjs.org).

<strong>Можно ли использовать бибилотеку компонентов?</strong>

Вы можете оформить проект как угодно, даже если это будет чистый **html** с минимальным количеством **css**, нам хватит. Если вы не можете не делать супер красивые и удобные интерфейсы, любите какой-то набор компонентов, можете воспользоваться им, например **Bootstrap**.

### <a name="api"></a> API

>Для запуска веб сервера выполните команду `npm start`

#### Получить список платформ

`GET /api/v1/platforms`

Ответ

```json
[  
    {  
        "label": "Desktop",
        "value": 1
    },
    {  
        "label": "Mobile",
        "value": 2
    }
]
```

#### Получить список браузеров

`GET /api/v1/browsers`

Пример ответа:

```json
[  
    {  
        "label": "Chrome",
        "value": 1,
        "platform": 1
    },
    {  
        "label": "Firefox",
        "value": 2,
        "platform": 1
    },
    {
        "..."
    }
]
```

#### Получить список операционных систем

`GET /api/v1/operating-systems`

Пример ответа:

```json
[  
     {  
        "label": "Windows",
        "value": 1,
        "platform": 1
    },
    {  
        "label": "Mac OS",
        "value": 2,
        "platform": 1
    },
    {
        "..."
    }
]
```

#### Получить список группировок

`GET /api/v1/groups`

Пример ответа:

```json
[  
    {  
        "label": "Day",
        "value": "day"
    },
    {  
        "label": "Platform",
        "value": "platform"
    },
    {
        "..."
    }
]
```

#### Получить статистику

`GET /api/v1/statistics?searchParams`

Ответ

```json
{
    "count": "количество всех записей на сервере по вашему запросу",
    "rows": "данные, с учетом пагинации",
    "total": "просуммированные показатели всех записей на сервере по вашему запросу"
}
```

Параметры `searchParams`

| Поле               | Тип                          | Обязательное | Значение по умолчанию | Описание                                                                                     |
|--------------------|------------------------------|--------------|-----------------------|----------------------------------------------------------------------------------------------|
| groupBy            | Строка                       | Да           | -                     | поле `value` из списка группировок                                                           |
| from               | Дата, в формате `YYYY-MM-DD` | Да           | -                     | дата с которой извлекаются события                                                           |
| to                 | Дата, в формате `YYYY-MM-DD` | Да           | -                     | дата по которую, включительно, извлекаются события                                           |
| limit              | Число                        | Нет          | 25                    | количество записей                                                                           |
| offset             | Число                        | Нет          | 0                     | сдвигает выборку на offset * limit записей, `offset=0&limit=25` возвращают первые 25 записей |
| platform           | Строка                       | Нет          | -                     | поле `value` из списка платформ                                                              |
| browsers[]         | Строка                       | Нет          | -                     | поле `value` из списка брузеров,,можно передать несколько                                    |
| operatingSystems[] | Строка                       | Нет          | -                     | поле `value` из списка операционных систем, можно передать несколько                         |

Несколько примеров

Получаем статистику за первые 7 дней июля, сгруппированную по дням

`GET /api/v1/statistics?groupBy=day&from=2019-07-01&to=2019-07-07`

Ответ

```json
{  
    "count": 7,
    "rows":[  
        {  
            "day": "2019-07-01",
            "impressions": 23,
            "clicks": 4,
            "money": 0.15445
        },
        {  
            "day": "2019-07-02",
            "impressions": 11,
            "clicks": 1,
            "money": 0.06262
        },
        {  
            "day": "2019-07-03",
            "impressions": 17,
            "clicks": 1,
            "money": 0.01448
        },
        {  
            "day": "2019-07-04",
            "impressions": 13,
            "clicks": 2,
            "money": 0.17215
        },
        {  
            "day": "2019-07-05",
            "impressions": 24,
            "clicks": 8,
            "money": 0.41402999999999995
        },
        {  
            "day": "2019-07-06",
            "impressions": 21,
            "clicks": 4,
            "money": 0.33422999999999997
        },
        {  
            "day": "2019-07-07",
            "impressions": 23,
            "clicks": 4,
            "money": 0.04693
        }
    ],
    "total":{  
        "impressions": 132,
        "clicks": 24,
        "money": 1.1988899999999998
    }
}
```

Вот так можем посмотреть что у нас происходило в июне и на разных платформах

`GET /api/v1/statistics?groupBy=platform&from=2019-06-01&to=2019-06-30`

Ответ

```json
{  
    "count": 2,
    "rows":[  
        {  
            "platform": "Mobile",
            "impressions": 473,
            "clicks": 97,
            "money": 4.42389
        },
        {  
            "platform": "Desktop",
            "impressions": 133,
            "clicks": 17,
            "money": 0.97717
        }
    ],
    "total":{  
        "impressions": 606,
        "clicks": 114,
        "money": 5.40106
    }
}
```

Получаем статистику за первые 7 дней июля, сгруппированную по дням, только по браузерам: Chrome (value=1), Firefox (value=2) и Chrome Mobile (value=5)

`GET /api/v1/statistics?groupBy=day&from=2019-07-01&to=2019-07-07&browsers[]=1&browsers[]=2&browsers[]=5`

Ответ

```json
{
    "count": 7,
    "rows": [
        {
            "day": "2019-07-01",
            "impressions": 2,
            "clicks": 0,
            "money": 0
        },
        {
            "day": "2019-07-02",
            "impressions": 4,
            "clicks": 2,
            "money": 0.10963
        },
        {
            "day": "2019-07-03",
            "impressions": 4,
            "clicks": 0,
            "money": 0
        },
        {
            "day": "2019-07-04",
            "impressions": 4,
            "clicks": 1,
            "money": 0.04082
        },
        {
            "day": "2019-07-05",
            "impressions": 6,
            "clicks": 1,
            "money": 0.01814
        },
        {
            "day": "2019-07-06",
            "impressions": 4,
            "clicks": 2,
            "money": 0.10832
        },
        {
            "day": "2019-07-07",
            "impressions": 9,
            "clicks": 1,
            "money": 0.01843
        }
    ],
    "total": {
        "impressions": 33,
        "clicks": 7,
        "money": 0.29534
    }
}
```

### The end

Баг фиксы и правки приветствуются. Все вопросы можно задать в [телеграмм](https://t.me/sse1595).

Пожалуйста, приложите инструкцию как нам запустить ваше решение.

Удачи!
