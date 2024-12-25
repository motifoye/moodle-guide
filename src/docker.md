# Развертывание Moodle в Docker

## Запуск используя docker-compose

### Образ от *bitnami*

```yaml
# Copyright Broadcom, Inc. All Rights Reserved.
# SPDX-License-Identifier: APACHE-2.0

services:
  mariadb:
    image: docker.io/bitnami/mariadb:11.4
    ports:
      - '8081:3306'
    environment:
      - MARIADB_ROOT_PASSWORD=moodle
      - MARIADB_USER=bn_moodle
      - MARIADB_PASSWORD=moodle
      - MARIADB_DATABASE=bitnami_moodle
      - MARIADB_PORT_NUMBER=3316
      - MARIADB_CHARACTER_SET=utf8mb4
      - MARIADB_COLLATE=utf8mb4_unicode_ci
    volumes:
      - 'mariadb_data:/bitnami/mariadb'
  moodle:
    image: docker.io/bitnami/moodle:4.5
    links:
      - mariadb
    ports:
      - '8080:8080'
      - '8443:8443'
    environment:
      - MOODLE_DATABASE_HOST=mariadb
      - MOODLE_DATABASE_PORT_NUMBER=3316
      - MOODLE_DATABASE_USER=bn_moodle
      - MOODLE_DATABASE_PASSWORD=moodle
      - MOODLE_DATABASE_NAME=bitnami_moodle
      - ALLOW_EMPTY_PASSWORD=yes
      # Установите максимальный размер загрузки файлов
      - PHP_UPLOAD_MAX_FILESIZE=1050M
      - PHP_POST_MAX_SIZE=1050M
    volumes:
      - C:\DockerData\bitnami\moodle:/bitnami/moodle
      - 'moodledata_data:/bitnami/moodledata'
      # - 'moodle_data:/bitnami/moodle'
    depends_on:
      - mariadb
volumes:
  mariadb_data:
    driver: local
#   moodle_data:
#     driver: local
  moodledata_data:
    driver: local
```

### Образ *Moodle-docker* от *MoodleHQ*

```yaml
# Copyright Broadcom, Inc. All Rights Reserved.
# SPDX-License-Identifier: APACHE-2.0

services:
  mariadb:
    image: docker.io/bitnami/mariadb:11.4
    ports:
      - '8081:3306'
    environment:
      # ALLOW_EMPTY_PASSWORD is recommended only for development.
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_USER=bn_moodle
      - MARIADB_DATABASE=bitnami_moodle
      - MARIADB_CHARACTER_SET=utf8mb4
      - MARIADB_COLLATE=utf8mb4_unicode_ci
    volumes:
      - 'mariadb_data:/bitnami/mariadb'
  moodle:
    image: docker.io/bitnami/moodle:4.5
    ports:
      - '8080:8080'
      - '8443:8443'
    environment:
      - MOODLE_DATABASE_HOST=mariadb
      - MOODLE_DATABASE_PORT_NUMBER=3306
      - MOODLE_DATABASE_USER=bn_moodle
      - MOODLE_DATABASE_NAME=bitnami_moodle
      # ALLOW_EMPTY_PASSWORD is recommended only for development.
      - ALLOW_EMPTY_PASSWORD=yes
    volumes:
      - 'moodle_data:/bitnami/moodle'
      - 'moodledata_data:/bitnami/moodledata'
    depends_on:
      - mariadb
volumes:
  mariadb_data:
    driver: local
  moodle_data:
    driver: local
  moodledata_data:
    driver: local
```

|Name              | Description             | Default Value |
|------------------|-------------------------|---------------|
|`MOODLE_USERNAME` | Moodle user name.       | user          |
|`MOODLE_PASSWORD` | Moodle user password.   | bitnami       |

**Монтирование каталогов** узлов в виде томов данных с помощью Docker Compose

Это требует незначительного изменения в `docker-compose.yml⁠` Файл, присутствующий в этом репозитории:

```diff
   mariadb:
     ...
     volumes:
-      - 'mariadb_data:/bitnami/mariadb'
+      - /path/to/mariadb-persistence:/bitnami/mariadb
   ...
   moodle:
     ...
     volumes:
-      - 'moodle_data:/bitnami/moodle'
+      - /path/to/moodle-persistence:/bitnami/moodle
-      - 'moodledata_data:/bitnami/moodledata'
+      - /path/to/moodledata-persistence:/bitnami/moodle
   ...
-volumes:
-  mariadb_data:
-    driver: local
-  moodle_data:
-    driver: local
```

[Докер хаб с докой](https://hub.docker.com/r/bitnami/moodle)

## Образ от MoodleHQ

[Docker Containers for Moodle Developers][mdlhqdckr] - Этот репозиторий содержит конфигурацию Docker, предназначенную для разработчиков и тестировщиков Moodle, чтобы легко развернуть тестовую среду для Moodle.

Для развертывания необходимо

1. Клонировать репозиторий Moodle желаемой [версии][mdlgittags]

   ```bash
   git clone --branch v4.5.0 --depth 1 --single-branch https://github.com/moodle/moodle.git moodle
   ```

2. Клонировать репозиторий с конфигурацией Docker

   ```bash
   git clone --depth 1 https://github.com/moodlehq/moodle-docker.git
   ```

3. В клонированном репозитории с конфигурацией необходимо создать файл, в котором следует определить переменные окружения. Список переменных представлен в таблице ниже.

   Только две из них обязательные. После объявления переменных запускается файл `bin\moodle-docker-compose.cmd` с аргументом `up` или `down`.

   Пример файла:

   ```cmd
   @echo OFF

   SET MOODLE_DOCKER_DB=mysql
   SET MOODLE_DOCKER_WWWROOT=<путь до файла>
   
   bin\moodle-docker-compose.cmd up
   ```

### Переменные окружения

Вы можете изменить конфигурацию образов docker, установив различные переменные среды **перед** вызовом `bin/moodle-docker-compose up`.
Когда вы измените их, используйте `bin/moodle-docker-compose down && bin/moodle-docker-compose up -d`, чтобы пересоздать вашу среду.


| Переменная среды                          | Обязательный | Допустимые значения                   | Стандартное   | Примечание                                                                    |
|-------------------------------------------|--------------|---------------------------------------|---------------|-------------------------------------------------------------------------------|
| `MOODLE_DOCKER_DB`                        | да           | pgsql, mariadb, mysql, mssql, oracle  | none          | Сервер базы данных, с которым нужно работать                                  |
| `MOODLE_DOCKER_WWWROOT`                   | да           | путь в вашей файловой системе         | none          | Путь к кодовой базе Moodle, которую вы собираетесь протестировать             |
| `MOODLE_DOCKER_DB_VERSION`                | нет          | Тег Docker - смотрите соответствующую страницу базы данных на docker-hub | mysql: 8.0 <br/>pgsql: 13 <br/>mariadb: 10.7 <br/>mssql: 2017-latest <br/>oracle: 21| Тег изображения в окне настройки сервера баз данных |
| `MOODLE_DOCKER_PHP_VERSION`               | нет          | 8.1, 8.0, 7.4, 7.3, 7.2, 7.1, 7.0, 5.6| 8.1           | Используемая версия php                                                       |
| `MOODLE_DOCKER_BROWSER`                   | нет          | firefox, chrome,  firefox:&lt;tag&gt;, chrome:&lt;tag&gt; | firefox:3       | Браузер, в котором будет запущен Behat. Поддерживает двоеточие, чтобы указать конкретную версию используемого образа Selenium docker. Например, firefox: 2.53.1 можно использовать для работы с более старыми версиями Moodle (< 3.5).              |
| `MOODLE_DOCKER_PHPUNIT_EXTERNAL_SERVICES` | нет          | любое значение                        | not set       | Если задано, будут добавлены зависимости для memcached, redis, solr и openldap|
| `MOODLE_DOCKER_BBB_MOCK`                  | нет          | любое значение                        | not set       | Если задано, то запускается и настраивается макет изображения BigBlueButton   |
| `MOODLE_DOCKER_MATRIX_MOCK`               | нет          | любое значение                        | not set       | Если задано, то запускается и настраивается макетное изображение матрицы      |
| `MOODLE_DOCKER_BEHAT_FAILDUMP`            | нет          | путь в вашей файловой системе         | not set       | Дампы сбоев Behat уже доступны по адресу http://localhost:8000/_/faildumps/ по умолчанию это позволяет сопоставлять определенную папку файловой системы для массового / автоматизированного извлечения дампов сбоев |
| `MOODLE_DOCKER_DB_PORT`                   | нет          | любое целочисленное значение          | none          | Если вы хотите привязаться к любому IP-адресу хоста, отличному от 127.0.0.1 по умолчанию, вы можете указать его в формате bind_ip:порт (0.0.0.0 означает привязку ко всем). Имя пользователя - "moodle" (или "sa" для mssql), а пароль - "m@0dl3ing". |
| `MOODLE_DOCKER_WEB_HOST`                  | нет          | any valid hostname                    | localhost     | Имя хоста для веб-сайта                                                       |
| `MOODLE_DOCKER_WEB_PORT`                  | нет          | любое целочисленное значение (or bind_ip:integer)| 127.0.0.1:8000| Номер порта для Интернета. Если установлено значение 0, порт не используется.<br/>Если вы хотите привязаться к любому IP-адресу хоста, отличному от 127.0.0.1 по умолчанию, вы можете указать его в формате bind_ip:порт (0.0.0.0 означает привязку ко всем). |
| `MOODLE_DOCKER_SELENIUM_VNC_PORT`         | нет          | любое целочисленное значение (or bind_ip:integer)| not set       | Если этот параметр установлен, узел selenium предоставит доступ к сеансу vnc на указанном порту. Аналогично MOODLE_DOCKER_WEB_PORT, вы можете дополнительно указать IP-адрес хоста для привязки. Если вы просто установите порт, VNC привязывается к 127.0.0.1 |
| `MOODLE_DOCKER_APP_PATH`                  | нет          | путь в вашей файловой системе         | not set       | Если этот параметр установлен и выбран браузер chrome, он запустит экземпляр приложения Moodle из вашей локальной кодовой базы |
| `MOODLE_DOCKER_APP_VERSION`               | нет          | версия [образа docker](https://docs.moodle.org/dev/Moodle_App_Docker_images) | not set       | Если установлено, запустится экземпляр приложения Moodle, если выбран браузер Chrome |
| `MOODLE_DOCKER_TIMEOUT_FACTOR`            | нет          | любое целочисленное значение          | 1             | Если этот параметр установлен, тайм-ауты в behat будут умножены на коэффициент|

[mdlhqdckr]: https://github.com/moodlehq/moodle-docker?ysclid=m3cmxixg6e869372127
[mdlgittags]: https://github.com/moodle/moodle/tags
[setup-docker-imgs]: https://moodledev.io/general/app/development/setup/docker-images
[mhq]: https://moodle.com/
