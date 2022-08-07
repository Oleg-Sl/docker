# ***Docker***

* [Install](#docker-install)
* [Container](#docker-container)
* [Images](#images)



## **DOCKER INSTALL**
[Официальная документация - install](https://docs.docker.com/engine/install/ubuntu/)
1. Обновите индекс apt-пакета и установите пакеты, чтобы разрешить apt использование репозитория по протоколу HTTPS:
```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
2. Добавление официального ключа GPG Docker:
```
 sudo mkdir -p /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
3. Команду для настройки docker репозитория:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
1. Обновление индекс apt-пакета и установите последней версии Docker Engine, containerd и Docker Compose:
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
5. Добавление своего пользователя в docker-группу.
[Официальная документация - postinstall](https://docs.docker.com/engine/install/linux-postinstall/)
```
sudo usermod -aG docker $USER
```


## **DOCKER CONTAINER**
*Список команд docker-container*
| Команда | Описание |
|---|---|
|docker ps	| Просмотр таблицы со всеми запущенными контейнерами
|docker ps -a | Просмотр таблицы со всеми остановленными контейнерами
|docker rm <*name_container*> | Удаление контейнера по имени - *name_container*
|docker rm <*container_id*> | Удаление контейнера по его идентификатору - *container_id*
|docker create --name <*name_container*> <*name_images*> | Создание контейнера с именем *name_container* из образа *name_images* (без запуска контейнера) [оф. док.](https://docs.docker.com/engine/reference/commandline/create/)
|docker run --name <*name_container*> -d <*name_images*>	| Создание и запуск контейнера с именем *name_container* из образа *name_images* (-d – для удаления привязки к текущей bash сессии, чтобы при ее завершении контейнер остался запущенным) [оф. док.](https://docs.docker.com/engine/reference/run/)
|docker stop <*name_container*>	| Остановка запущенного контейнера с именем *name_container* (посфлается сигнал – SIGTERM, если контейнер не останавливается - SIGKILL)
|docker start <*name_container*> | Запуск контейнера с именем *name_container*
|docker restart <*name_container*> | Перезапуск контейнера с именем *name_container*
|docker pause <*name_container*> | Приостановка контейнера с именем *name_container* (сигнал – SIGSTOP)
|docker unpause <*name_container*> | Снятие с паузы контейнера с именем *name_container* 
|docker kill <*name_container*> | Убить контейнер с именем *name_container* (сигнал – SIGKILL)
|docker kill –signal=<*number_signal*> <*name_container*> | Послать сигнал *number_signal* для остановки контейнера с именем *name_container*
|docker container prune	| Удаление всех остановленных контейнеров
|docker rename <*old_name_container*> <*new_name_container*> | Переименование контейнера с именем *old_name_container* на имя *new_name_container*
|docker stats | Просмотр информации о запущенных контейнерах
|docker inspect <*name_container*> | Подробная информация о контейнере с именем *name_container* (при добавлении флага –s к выводу добавляется переменная *SizeRootFs* показывающая сколько места занимает контейнер на диске)
|docker inspect –f “.<*field_name*>.<*field_name*>.…” <*name_container*> | Получить информацию из поля *field_name*
|docker logs <*name_container*> | Просмотр списка логов контейнера (добавление флага –f позволяет выводить логи в консоль в реальном времени)
|docker exec [параметры] <*name_container*> [команда] | Параметры: <br>-i – интерактивный ввод;<br>-t – псевдо tty (по сути ssh-ся в контейнер);<br>-d – запуск в фоне (вне текущего процесса bash);<br>-e – добавление переменной окружения в контейнер;<br>-u – пользователь относительно которого хотим выполнить команду;<br>-w – рабочая директория.<br>Команда Linux: bash, pwd, printenv и др. команды Linux исполняемые внутри контейнера

Можно отфильтровать список логов используя команды Linux:
```
grep “<str>” –A <number_rows>
```
, где *str* - строка поиска, *number_rows* - количество выводимых строк, возможные варианты флага:
    -A – *number_rows* строк после вхождения; 
    -B – *number_rows* строк перед вхождением; 
    -m – *number_rows* первых строк.

## **DOCKER IMAGES**




