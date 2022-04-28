[![Garant API CI/CD PROD](https://github.com/GarantLogistika/API_GL/actions/workflows/api_im_prod.yml/badge.svg)](https://github.com/GarantLogistika/API_GL/actions/workflows/api_im_prod.yml)


API транспортной компании Гарант-Логистика.
=
Здесь наши клиенты, а именно юридические лица могут оставлять свои заявки на доставку\
заказов из своих магазинов, складов и пр. по большей части территории Российской Федерации.

Технологии, используемые на проекте: \
Backend:
1. Python >= v.3.10 ![Python](https://img.shields.io/badge/-Python-black?style=flat-square&logo=Python)
2. FastAPI ![FastAPI](https://img.shields.io/badge/-FastAPI-%2300C7B7?style=flat-square&logo=FastAPI)
3. PostgresSQL ![Postgresql](https://img.shields.io/badge/-Postgresql-%232c3e50?style=flat-square&logo=Postgresql)
4. pgAdmin ![pgAdmin](https://img.shields.io/badge/PG-pgAdmin-blue?style=flat-square&logo=pgAdmin)
5. asyncpg ![asyncpg](https://img.shields.io/badge/-asyncpg-blue?style=flat-square&logo=Postgresql)
6. Celery ![Celery](https://img.shields.io/badge/-Celery-%2300C7B7?style=flat-square&logo=Celery)
7. Flower ![Flower](https://img.shields.io/badge/F-Flower-green?style=flat-square&logo=Celery)
8. Redis ![Redis](https://img.shields.io/badge/-Redis-FCA121?style=flat-square&logo=Redis)
9. aioredis ![aioredis](https://img.shields.io/badge/-aioredis-critical?style=flat-square&logo=Redis)
10. yoyo-migrations ![yoyo-migrations](https://img.shields.io/badge/yoyo-migrations-9cf?style=flat-square&logo=Postgresql)
11. Nginx ![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=flat-square&logo=nginx&logoColor=white)

Tools:
1. Docker ![Docker](https://img.shields.io/badge/-Docker-46a2f1?style=flat-square&logo=docker&logoColor=white)
2. Docker-compose ![docker-compose](https://img.shields.io/badge/docker-compose-9cf?style=flat-square&logo=docker)


Как запустить проект:
=
В корневую папку проекта расположить .env файл со следующими параметрами:

1. SECRET_KEY=***ВАШ СЕКРЕТНЫЙ КЛЮЧ***
2. POSTGRES_HOST=***ИМЯ ХОСТА, НАЗНАЧЕННОЕ В SERVICES DOCKER-COMPOSE, в нашем случае следует установить имя postgresql_db***
3. POSTGRES_USER=***ИМЯ ПОЛЬЗОВАТЕЛЯ POSTGRESQL***
4. POSTGRES_PASSWORD=***ПАРОЛЬ ОТ POSTGRESQL***
5. POSTGRES_PORT=***ПОРТ, ПО УМОЛЧАНИЮ 5432***
6. POSTGRES_DB=***БАЗА ДАННЫХ POSTGRESQL***
7. PGADMIN_DEFAULT_EMAIL=***ВАША ПОЧТА ОТ PGADMIN***
8. PGADMIN_DEFAULT_PASSWORD=***ВАШ ПАРОЛЬ ОТ PGADMIN***
9. URL_EXPIRE_HOURS=***ВРЕМЯ ЖИЗНИ ССЫЛОК***
10. ACCESS_LOGIN_1C=***ЛОГИН К 1С***
11. ACCESS_PASSWORD_1C=***ПАРОЛЬ К 1С***
12. ACCESS_TOKEN_1C=***ТОКЕН ДОСТУПА К 1С***
13. SEARCH_COMPANY_URL_1C=***URL ПОИСКА ОРГАНИЗАЦИЙ В 1С***
14. API_1C_URL=***ОСНОВНОЙ URL К 1С***
15. FROND_URL=***URL ФРОНТА***
16. REDIS_PASSWORD=***ПАРОЛЬ ОТ REDIS***
17. REDIS_URL=***URL ДО REDIS***
18. REDIS_EXPIRE_SEC=***ВРЕМЯ ЖИЗНИ КЕША REDIS, В СЕКУНДАХ***
19. CELERY_BROKER_URL=***URL ДО БРОКЕРА CELERY***
20. CELERY_RESULT_BACKEND=***URL ДО ХРАНИЛИЩА РЕЗУЛЬТАТОВ CELERY***
21. FLOWER_USER=***ИМЯ ПОЛЬЗОВАТЕЛЯ FLOWER***
22. FLOWER_PASSWORD=***ПАРОЛЬ FLOWER***

Либо скачать с корпоративного FTP [сайта](https://garant-logistic-ftp-client.tk-gl.ru/) файл .env \
Название файла что-то вроде ***ПЕРЕМЕННЫЕ ОКРУЖЕНИЯ API ГАРАНТ-ЛОГИСТИКА***

Скачать docker: 
1. Для [windows](https://docs.docker.com/desktop/windows/install/)
2. Для [macOS](https://docs.docker.com/desktop/mac/install/)
3. Для дистрибутивов [Linux](https://docs.docker.com/desktop/linux/#uninstall)

После установки проверьте конфигурацию переменных окружений\
командой:
```
docker-compose config
```
Если всё успешно, все переменные на местах, запустить командой:

```
Запуск на VPS: docker-compose up --build -d
```
```
Запуск в режиме разработки(localhost): docker-compose -f docker-compose.dev.yml up --build -d
```

Что бы произвести миграции в базу данных,
необходимо войти в контейнер командой:
```
docker exec -it ИМЯ КОНТЕЙНЕРА, в нашем случаение это asyncpg_backend
```
После ввести команду:
```
python scripts/db_manager.py --migrate
```
По завершению в консоль должна вывестись команда ***Успешно***

Вот и всё! Можно приступать к работе. Никаких дополнительных действий совершать\
не нужно, все изменения в контейнеры подтягиваются автоматически.

:exclamation:Важно! Для внесения и применения изменений в задачи(tasks) Celery\
необходим перезапуск контейнера:exclamation:

Сделать это можно командами:
```
docker-compose restart
```
Эта команда перезапустит все контейнеры разом.

Для перезапуска отдельного контейнера используйте команду:
```
docker-compose restart ИМЯ СЕРВИСА, в нашем случае нам нужно перезапустить CELERY
и FLOWER.
Выполните их поочередно, сначала docker-compose restart celery_worker,
после docker-compose restart flower
```

Все сервисы доступны по следующим адресам:
1. FastAPI(Само приложение) $.$.$.$:9000
2. Мониторинг POSTGRESQL: pgAdmin $.$.$.$:5050
3. Мониторинг Celery: Flower $.$.$.$:5555

Что реализовано на проекте:
=
1. Основная логика идентификации, аутентификации и авторизации.
2. Сброс и смена пароля.
3. Составление заказов.

Что нужно доделать:
=
1. Реализовать обновление, отмены и пр. логику, связанную с заказами. Пример можно посмотреть [тут](https://pecom.ru/easyway/api/doc/).
2. Реализовать взаимодействие backend и frontend сервисов.
3. Покрыть проект тестами.
4. Настроить CI/CD GitHub actions для комфортной развертки на VPS
5. Настроить сервис Nginx под ваши нужны. В данный момент он отключен.

![alt text](https://tk-gl.ru/static/logo_title.png)
