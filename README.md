# Сервис ранжирования мероприятий

Данный документ содержит описание работы и информацию о развертке микросервиса, предназначенного для ранжирования мероприятий для пользователя в соответствии с его интересами.

Название сервиса: `ranking_service`

Структура сервиса:

| Файл                 | Описание                                                     |
| -------------------- | ------------------------------------------------------------ |
| `ranking_service.py` | Код микросервиса                                             |
| `config.yml`         | Конфигурационный файл со строкой подключения к RabbitMQ      |
| `run.sh`             | Файл для запуска сервиса из Docker контейнера                |
| `requirements.txt`   | Верхнеуровневые зависимости                                  |
| `Dockerfile`         | Описание сборки контейнера сервиса                           |
| `docker-compose.yml` | Изолированная развертка сервиса вместе с (RabbitMQ, MongoDB) |
| `README.md`          | Описание микросервиса                                        |

## API

### RPC

Составить или изменить список мероприятий, интересных пользователю

```bat
n.rpc.ranking_service.change_top_user([user_id, user_tags])

Args:
    list of two values -
        user_id
        user_tags - dict like {'tag_1': w_1, ..., 'tag_m': w_m}
Returns nothing
Sendind  user's top to `top_das`
```

Составить или изменить список мероприятий, интересных пользователю - для всех пользователей

```bat
n.rpc.ranking_service.change_top_all(plug)

Args:
    plug - can be anything (is used only due to event_handler's syntaxis)
Returns nothing
Call for ranking_service.change_top_user for all user in db
```

## Развертывание и запуск

### Локальный запуск

Для локального запуска микросервиса требуется запустить контейнер с RabbitMQ.

```bat
docker-compose up -d
```

Затем из папки микросервиса вызвать

```bat
nameko run ranking_service
```

Для проверки `rpc` запустите в командной строке:

```bat
nameko shell
```

После чего откроется интерактивная Python среда. Обратитесь к сервису одной из команд, представленных выше в разделе `rpc`

### Запуск в контейнере

Чтобы запустить микросервис в контейнере вызовите команду:

```bat
docker-compose up
```

> если вы не хотите просмотривать логи, добавьте флаг `-d` в конце
