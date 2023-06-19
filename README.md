# slurm_ansible_practicum
Практикум по курсу Ansible в рамках курса DevOps Upgrade от Slurm

## Задание:

**Цель:** Закрепление материала полученного в курсе Ansible, получение практического опыта написания и использования ролей.

**Результат:** Работающая инфраструктура для приложения. Репозиторий на Github.com с ролями и плейбуками Ansible.

**Задание:** Необходимо подготовить сервер для работы приложения на основе ОС Centos 7.

-   Само приложение доступно в репозитории  [https://gitlab.slurm.io/edu/xpaste_practicum.](https://gitlab.slurm.io/edu/xpaste_practicum.)
-   Приложение написано на Ruby, должно запускаться как сервис в systemd (сервис файл для приложения нужно подготовить самостоятельно).
-   Настройки для приложения должны передаваться как переменные окружения в сервис файле.
-   Набор переменных необходимых приложения для работы:

-   SECRET_KEY_BASE - любая строка
-   RAILS_ENV=production
-   RAILS_LOG_TO_STDOUT=1
-   DB_HOST - адрес хоста postgresql (127.0.0.1)
-   DB_PORT - порт postgresql (5432)
-   DB_NAME - имя базы данных
-   DB_USER - пользователь в базе данных
-   DB_PASSWORD - пароль для базы данных

-   Для деплоя приложения на сервер нужно подготовить отдельную Ansible роль, которая должна включать установку зависимостей для приложения, деплой сервис файла из шаблона, деплой кода самого приложения, запуск приложения.

-   Для установки зависимостей приложения требуются следующие пакеты:

-   build-base
-   libxml2-dev

-   Для установки самих зависимостей нужно выполнить:

-   bundle config build.nokogiri --use-system-libraries
-   bundle install --clean --no-cache --without development

-   Для запуска приложения в сервис файле нужно выполнять команды:

-   bundle exec rake db:migrate && \
-   bundle exec puma -b unix:///var/run/puma.sock -e $RAILS_ENV config.ru

-   Приложению для работы необходима БД PostgreSQL. Для разворачивания базы данных нужно подготовить собственную ansible роль. Роль должна включать в себя установку и настройку Postgres, а также создание пользователя с паролем и базы данных для приложения.
-   Перед приложением необходимо установить балансировщик Nginx. Роль для балансировщика нужно взять готовую из набора Galaxy. Конфигурация для Nginx находится в файле config/nginx.conf. Самостоятельно исправьте локацию с обращением на бэкенд, для правильной работы приложения.

**Практические рекомендации:**

-   Для выполнения задания достаточно одной виртуальной машины. Запустите все службы (приложение, Nginx и PostgreSQL) на ней.
