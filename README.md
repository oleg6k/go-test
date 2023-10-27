# Тестовое задание для GoLang разработчика

## Описание задания

Необходимо реализовать сервис, который предоставляет HTTP API для загрузки 
файлов с конфиграциями и расчета результата на основе этих конфигураций.

> Все конфигурации загружаются в формате `json`.

Имеется три типа конфигураций, которые загружаются в сервис:

* **reels** - конфигурация с описанием игрового поля. Представляет собой 
матрицу 3x5, которая заполнена символами от `A` до `G` (7 символов). Пример:
```json
[
  ["A", "B", "C", "D", "E"],
  ["F", "A", "F", "B", "C"],
  ["D", "E", "A", "G", "A"]
]
```
* **lines** - конфигурация с описанием игровых линий. Представляет собой массив 
объектов, где каждый объект описывает линию и содержит номер линии и массив
объектов с позициями в линии. Пример:
```json
[
    {
        "line": 1, 
        "positions": [
            {"row": 0, "col": 0},
            {"row": 1, "col": 1},
            {"row": 2, "col": 2},
            {"row": 1, "col": 3},
            {"row": 0, "col": 4}
        ]
    },
    {
        "line": 2, 
        "positions": [
            {"row": 2, "col": 0},
            {"row": 1, "col": 1},
            {"row": 0, "col": 2},
            {"row": 1, "col": 3},
            {"row": 2, "col": 4}
        ]
    },
    {
        "line": 3, 
        "positions": [
            {"row": 1, "col": 0},
            {"row": 2, "col": 1},
            {"row": 1, "col": 2},
            {"row": 0, "col": 3},
            {"row": 1, "col": 4}
        ]
    }
]
```
* **payouts** - конфигурация с описанием выигрышных комбинаций. Представляет собой 
массив объектов, где каждый объект описывает символ и массив выигрышей для
количества повторов символа в линии. Пример:
```json
[
    {
        "symbol": "A",
        "payout": [0, 0, 50, 100, 200]
    },
    {
        "symbol": "B",
        "payout": [0, 0, 40, 80, 160]
    },
    {
        "symbol": "C",
        "payout": [0, 0, 30, 60, 120]
    },
    {
        "symbol": "D",
        "payout": [0, 0, 20, 40, 80]
    },
    {
        "symbol": "E",
        "payout": [0, 0, 10, 20, 40]
    },
    {
        "symbol": "F",
        "payout": [0, 0, 5, 10, 20]
    },
    {
        "symbol": "G",
        "payout": [0, 0, 2, 5, 10]
    }
]
```

На основе загруженных конфигураций сервис должен реализовывать расчет результата 
по следующему алгоритму:

* Проиводим обход всех линий из конфигурации **lines**;
* Для каждой линии выбираем символы из конфигурации **reels** по позициям из 
линии. Если возмем первую линию из примера выше, то получим следующие символы
[`A`, `A`, `A`, `B`, `E`];
* Проверяем сколько повторов у символа в линии, который находится на первой 
позиции в этой линии. Если у нас на первой позиции в линии символ `A` и он идет
с лева на право подряд три раза то количество повторов будет равно 3;
* Находим в конфигурации **payouts** выигрыш для символа c учетом количества 
повторов символа в линии. Если у нас на первой позиции в линии символ `A` и он 
идет с лева на право подряд три раза то количество повторов будет равно 3 и 
выигрыш будет равен 50;

## Описание необходимого функционала

Сервис должен реализовывать следующий функционал:

* Загрузка конфигураций в сервис по HTTP протоколу. При загрузке конфигурации 
должны указываться тип конфигурации и имя конфигурации;
* Обслуживание HTTP запроса на расчет результата на основе загруженных 
конфигураций. В запросе должны указываться имена конфигураций, которые 
необходимо использовать для расчета результата. Ответ должен содержать 
результат выигрыша по каждой линии и общий результат. Пример ответа:
```json
{
    "lines": [
        {"line": 1, "payout": 50},
        {"line": 2, "payout": 0},
        {"line": 3, "payout": 0}
    ],
    "total": 50
}
```
* Обслуживание HTTP запросов к /metrics в формате prometheus.

> Методы HTTP API должны возвращать ответ в формате `json`.

Метрические данные должны включать в себя как минимум следующие метрики:

* Общее количество обработанных запросов к API-endpoints;
* Количество ошибок обработки HTTP запросов к API-endpoints;
* Данные по времени обработки HTTP запросов к API-endpoint;

## Требования к реализации
 
* Все функции _(экспортируемые и не экспортируемые)_ должны сопровождаться 
понятным комментарием;
* Не стоит излишне сокращать имена переменных и констант - код пишется для людей, 
и он должен быть максимально простым и понятным;
* Можно использовать любые сторонние пакеты, но не использовать какой-либо фреймворк;
* Весь ключевой функционал должен быть зафиксирован unit-тестами;
* После завершения работы над заданием необходимо написать сопроводительную 
документацию по работе с приложением в файле `README.md` в корне репозитория;

## Плюсами будут являться

- Настройка CI _(силами GitHub actions)_ выполняющая запуск тестов и сборки 
на каждый коммит;
- Автоматическая сборка Docker-образа с приложением;
- Интуитивно-понятное разбитие коммитов - одной конкретной задаче - один 
коммит или PR (её правки - отдельный коммит или PR);

## Результат выполнения

Ссылку на репозиторий с вашей реализацией необходимо отправить нашему 
HR или TeamLead, от которого вы получили ссылку на данный репозиторий.

Приложение должно успешно запускаться после выполнения:

```bash
$ git clone ...
$ make build
```

> Если для запуска приложения потребуется другой набор команд - обязательно 
> отразите это в файле `README.md` вашего репозитория.

Так же должны выполняться unit-тесты с помощью команды:

```bash
$ make test
# and optionally:
$ make cover
```
