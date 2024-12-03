#  Проект "Kittygram"

![example workflow](https://github.com/AlexanderTryapyshko/kittygram_final/actions/workflows/main.yml/badge.svg)

Позволяет собрать коллекцию разнообразных котов.

### Стек использованных технологий.

Бэкенд проекта написан на языке Python при помощи фреймворков Django REST Framework и React

### Как запустить проект: 

* Клонировать репозиторий и перейти в него в командной строке: 

    ``` 
    git clone git@github.com:AlexanderTryapyshko/kittygram_final.git
    ``` 

* В разделе settings - secrets and variables - actions создать секреты:

    - DOCKER_USERNAME - Ваш логин на Docker Hub

    - DOCKER_PASSWORD - Ваш пароль на Docker Hub

    - USER - ваше имя пользователя на сервере

    - HOST - IP-адрес вашего сервера

    - SSH_KEY - содержимое екстового файла с закрытым SSH-ключом

    - SSH_PASSPHRASE - passphrase от сервера

    - TELEGRAM_TOKEN - токен телеграмм-бота

    - TELEGRAM_TO - id телеграмм-пользователя, которому должны приходить уведомления об успешном деплое проекта

* Перейти в директорию проекта 
    
    ``` 
    cd kittygram_final 
    ``` 

* Cоздать и активировать виртуальное окружение: 
    
    ``` 
    python3 -m venv env 
    ``` 

    -  Если у вас Linux/macOS 

        ```
        source env/bin/activate 
        ```
        
    - Если у вас windows 

        ```
        source env/scripts/activate 
        ```

* Запусить Docker Compose:

    ```
    docker compose -f docker-compose.production.yml up --build
    ```

* Собрать статику:

    ```
    docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic

    docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
    ```

* Выполнить миграции:

    ```
    docker compose -f docker-compose.production.yml exec backend python manage.py migrate
    ```

* В корне проекта создать файл .env, в котором указать данные для взаимодействия с PostgreSQL:

    - POSTGRES_USER - имя пользователя БД (необязательная переменная, значение по умолчанию — postgres)

    - POSTGRES_PASSWORD — пароль пользователя БД (обязательная переменная для создания БД в контейнере)

    - POSTGRES_DB — название базы данных (необязательная переменная, по умолчанию совпадает с POSTGRES_USER)

    - DB_NAME - имя БД

    - DB_HOST — адрес, по которому Django будет соединяться с базой данных.

    - DB_PORT — порт, по которому Django будет обращаться к базе данных. 5432 — это порт по умолчанию для PostgreSQL

    - SECRET_KEY - значение SECRET_KEY Django-проекта

* В настройках settings.py проекта в ALLOWED_HOSTS указать ip-адрес сервера и домен

* В файле docker-compose.production в services указать адреса образов на docker hub, по которым эти образы буду собираться в формате username/imagename:tag, где:

    - username — ваше имя пользователя на Docker Hub

    - imagename — произвольное имя образа

    - tag - версия образа (по умолчанию - latest)

* Скопировать env-file на сервер:

    ```
    scp -i path_to_SSH/SSH_name .env username@server_ip:/home/username/kittygram/.env
    ```

    где:

    - path_to_SSH — путь к файлу с SSH-ключом

    - SSH_name — имя файла с SSH-ключом (без расширения)

    - username — ваше имя пользователя на сервере

    - server_ip — IP вашего сервера

* В настройках nginx на сервере указать:

    - server_name - адрес сайта

* Отправить изменения на GitHub:

    - командой ``` git add --all ``` добавить изменения

    - командой ``` git commit -m 'commit text' ``` создать коммит

    - командой ``` git push ``` отправить изменения на GitHub

### Принцип работы:

- Чтобы просмотреть уже существующих котов, необходимо авторизоваться на сайте.

- Чтобы добавить кота, необходимо на главной странице сайта в правом верхнем углу нажать кнопку "добавить кота".

- После этого открывается окно добавления котика. В нем необходимо добавить изображение котика, заполнить имя и год рождения, выбрать достижение (уже существующее или создать новое) и цвет котика, после чего нажать "Сохранить". 

- Так же на странице котика можно отредактировать информацию либо удалить котика.

##### Автор проекта - Александр Тряпышко.
