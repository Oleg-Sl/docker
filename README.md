# ***Docker***

- [***Docker***](#docker)
  - [**DOCKER INSTALL**](#docker-install)
  - [**DOCKER CONTAINER**](#docker-container)
  - [**DOCKER IMAGES**](#docker-images)
  - [**DOCKERFILE**](#dockerfile)
  - [Анализ образа и поиск способов уменьшения его размера](#анализ-образа-и-поиск-способов-уменьшения-его-размера)
  - [**DOCKER NETWORK**](#docker-network)
    - [**Bridge**](#bridge)
    - [**Host**](#host)
    - [**Overlay**](#overlay)
    - [**Macvlan**](#macvlan)
    - [**None**](#none)
  - [**VOLUMES**](#volumes)
  - [volume](#volume)
  - [bind mount](#bind-mount)
  - [tmpfs mount](#tmpfs-mount)

[Base Command](https://docs.docker.com/engine/reference/commandline/docker/)

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
*Список команд docker-images*
| Команда | Описание |
|---|---|
|docker pull <*name_images*> | Скачивание образа <*name_images*> из удаленного репозитория
|docker push <*name_images*> | Отправка образа *name_images* в удаленный репозиторий
|docker images | Просмотр всех скачанных образов
|docker save –output <*name_archive*> <*name_images*> | Скачивание образа *name_images* в архив *name_archive*
|docker images import <*name_archive*> | Создание образа из архива *name_archive*
|docker history <*name_images*> | История создания образа *name_images*
|docker inspect <*name_images*> | Вся информация об образе *name_images*
|docker images ls | Просмотр всех скачанных образов
|docker images ls ––format {{.*Tag_name*}} | Просмотр скачанных образов с тэгом *Tag_name*
|docker images ls ––filter {{before=<*name_images*>}} | Просмотр скачанных образов с именем *name_images*
|docker image rm <*name_images*> <br> docker rmi <*name_images*> | Удаление образа с именем *name_images*
|docker tag <*name_images_actual*>:<*tag_name_actual*> <*name_images_new*>:<*tag_name_new*>	| Создание нового образа *name_images_new*:*tag_name_new* на основе исходного *name_images_actual*:*tag_name_actual* (полное копирование)
|docker images prune | Удаление всех образов у которых тэг равен «none»

## **DOCKERFILE**
[Dockefile documentation](https://docs.docker.com/engine/reference/builder/)

Docker может создавать образы автоматически, читая инструкции из Dockerfile. 
Dockerfile - это текстовый документ, содержащий все команды, которые пользователь может вызвать в командной строке для сборки образа. С помощью docker build этого приложения пользователи могут создавать автоматическую сборку, которая последовательно выполняет несколько инструкций командной строки.

Для создания образа из Dockerfile вызвать команду:
```
docker build [flag] <*context*>
```
где *context* - это директория со всеми вложеными директориями и файлами (для исключения директорий и файлов из контекста добавить их в файл *.dockerignore*); наиболее часто используемые флаги:
- f – путь к Dockefile;
- t – указание имени создаваемого образа «имя:тэг»;

*Список инструкций Dockerfile*
| Инструкция | Описание |
|---|---|
|ARG <*name*>[=<*default value*>] | Аргумент сборки – определяет переменную, которая передается во время сборки сборщику docker build с помощью команды, использующей --build-arg <*varname*>=<*value*> флаг
|FROM <*name_images*> [AS <*other_name*>] | Инициализирует новый этап сборки и устанавливает базовый образ для последующих инструкций. Если образ не на чем не должен базироваться – FROM skretch
|ONBUILD <*instruction*> | Добавление к образу команды запуска, которая будет выполнена позже, когда образ будет использоваться в качестве основы для другой сборки.
|LABEL <*key*>=<*value*> <*key*>=<*value*> ... | Добавление мета информацик к образу
|USER <*user*>[:<*group*>] | Установка пользователя от имени которого будут выполняться дальнейшие команды текущего этапа
|WORKDIR <*paath*> | Установка директории относительно которой будут выполняться дальнейшие команды текущего этапа
|ADD [--chown=<user>:<group>] <*src*>... <*dest*> | Копирование каталогов и файлов <*src*> в файловую систему образа по пути <*dest*>. Можно передать архив и папку для его разархивирования
|COPY [--chown=<user>:<group>] <*src*>... <*dest*> | Копирование каталогов и файлов <*src*> в файловую систему образа по пути <*dest*>. Позволяет копировать из предыдущих образов при multistagebuild
|SHELL ["executable", "parameters"] | Переопределение командной оболочки по умолчанию. Например: ```SHELL [“/bin/sh”, “-c”]`
|RUN <*command*> | Выполнение команды в новом слое поверх текущего образа и зафиксирует результаты. Полученный зафиксированный образ будет использоваться для следующего шага в Dockerfile
|ENV <*key*>=<*value*> ... | Установка переменной среды <*key*> в значение <*value*>. Это значение будет в среде для всех последующих инструкций на этапе сборки и может быть заменено.
|VOLUME ["*/data*"] |	Создание точки монтирования с указанным именем и пометка ее как содержащую подключенные извне тома из собственного хоста или других контейнеров.
|CMD ["*executable*","*param1*","*param2*"] | Команда которая будет выполнена после запуска контейнера из этого образа
|ENTRYPOINT ["executable", "param1", "param2"] | Команда которая будет выполнена после запуска контейнера из этого образа
|STOPSIGNAL <*signal*> | Задание сигнала системного вызова, который будет отправлен в контейнер для завершения. Этот сигнал может быть именем сигнала в формате SIG<*NAME*>, например SIGKILL, или числом без знака, которое соответствует позиции в таблице системных вызовов ядра. Значение по умолчанию - SIGTERM.
|EXPOSE <*port*> [<*port*>/<*protocol*>...] | Cообщение, что контейнер прослушивает указанные сетевые порты во время выполнения (не прокидывает порты, используется только для документирования) 
|# Comment | Комментарии

## Анализ образа и поиск способов уменьшения его размера
Утилита [Dive](https://github.com/wagoodman/dive)<br>
Установка *Dive*:
```
wget https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.deb
sudo apt install ./dive_0.9.2_linux_amd64.deb
```
Запуск *Dive*
```
dive <name_images:tag_images>
```

## **DOCKER NETWORK**
[Network - оф. док.](https://docs.docker.com/network/)

Сеетями в docker управляет утилита *Libnetwork*, выполняет:
- Балансировку нагрузки
- Управление сетями
- Service discovery (чтобы один контейнер находил другой)
Эта библиотека работает на основе инструментов предоставляемых Linux ядром:
- Network Namespace
- Linux Bridge
- Virtual Ethernet Devices
- IP Tables
Типы сетевых driver Docker:
- [bridge](#bridge) - изолированная сеть между контейнерами
- [host](#host) - удаление изоляции контейнера
- [overlay](#overlay) - docker swarm
- [macvlan](#macvlan) - уникальный MAC адресс для контейнера
- [none](#none) - без сети

*Список инструкций управления сетями Docker*
| Инструкция | Описание |
|---|---|
|docker network connect <*name_network*> <*name_container>*|Подключение контейнера к сети
|docker network create <*name_network*>| Создать сеть
|disconnect| Отключение контейнера от сети
|docker network inspect <*name_network*>| Параметры сети
|docker network ls| Список всех сетей
|docker network prune| Удаление всех сетей
|docker network rm <*name_network*>| Удаление сети
Изменение DNS сервера используемого контейнером на этапе создания контейнера выполняется добавлением флага --dns <ip_addr>:
```
docker run --name <name_network> -d --dns <ip_addr> <name_images>
```
Используемый DNS сервер указан в файле конфигурации:
```
/etc/resolv.conf
```

### **Bridge**
Это золированная сеть между контейнерами.
Работает только на 1 хосте. Сеть с именем и типом *bridge* поднимается по умолчанию.
Создание новой подсети:
```
docker network create <name_network>
```
Подключение контейнера к созданной сети:
```
docker network connect <name_network> <name_container>
```
Создание контейнера с указанием его единственной сети:
```
docker run --network <name_network> --name <name_container> -d <name_images>
```
Создание контейнера с пробросом портов с хост машины внутрь контейнера:
```
docker run --network <name_network> -p <port_host:port_container> --name <name_container> -d <name_images>
```

### **Host**
Удаляется изоляция контейнера от хостовой сети и работает с ней напрямую.
Команда создания контейнера с сетью типа host:
```docker
docker run --network host --name <name_container> -d <name_images>
```

### **Overlay**

### **Macvlan**

### **None**
Исполльзуется при отсутсвии необходимости сети в контейнере.
Команда создания контейнера без сети:
```docker
docker run --name <name_container> -d --network none <name_images>
```

## **VOLUMES**
Это механизм сохранения данных, генерируемых docker контейнером, после его остановки и созданием нового контейнера.
Для чего используются тома:
- шаринг данных между несколькими запущенными контейнерами
- решение проблемы привязки к ОС хоста
- удалённое хранение данных
- бэкап или миграция данных на другой хост с Docker
Существуют три способа мотнтирования томов:
- volume
- bind mount
- tmpfs mount



## volume
Подключение области хостовой машины к контейнеру.
Тома находятся по умолчанию в /var/lib/docker/volumes/. Другие программы не должны получать к ним доступ напрямую, только через контейнер.
Команды volume контейнера:

```docker
docker volume create <name_volume>
docker volume inspect <name_volume>
docker volume ls
docker volume prune
docker volume rm
```


## bind mount
Файл или каталог с хоста просто монтируется в контейнер
Используется, когда нужно пробросить в контейнер конфигурационные файлы с хоста. Другое очевидное применение — в разработке. Код находится на хосте (вашем ноутбуке), но исполняется в контейнере. Вы меняете код и сразу видите результат.
Особенности bind mount:
- Запись в примонтированный каталог могут вести программы как в контейнере, так и на хосте
- Лучше не использовать в продакшене. Для продакшена убедитесь, что код копируется в контейнер, а не монтируется с хоста.
- Для успешного монтирования указывайте полный путь к файлу или каталогу на хосте.
- Если приложение в контейнере запущено от root, а совместно используется каталог с ограниченными правами, то в какой-то момент может возникнуть проблема с правами на файлы и невозможность что-то удалить без использования sudo.

## tmpfs mount
Tmpfs монтрирование является временным и сохраняется в оперативной памяти хоста. Когда контейнер останавливается, tmpfs монтирование удаляется и файлы, записанные в нем, не сохраняются.
Совместное использование tmpfs монтирования между контейнерами не поддерживается.
Команды tmpfs монтирования контейнера:
```docker
docker run -d -it --name <name_container> --tmpfs <path> <name_image:tag>
docker run -d -it --name <name_container> --mount type=tmpfs,destination=<path> <name_image:tag>
```
В сервисах *swarm* ипользуется только флаг *--mount*

