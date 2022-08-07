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
|docker ps -a	| Просмотр таблицы со всеми остановленными контейнерами
|docker rm <name_container> | Удаление контейнера по имени - name_container
|docker rm <container_id> | Удаление контейнера по его идентификатору - container_id
|docker create --name <имя> <имя_контейнера_в_репозитории>	| Создание контейнера
|docker run -d <контейнер> | 
|docker run --name <имя> -d <контейнер>	| Создание и запуск контейнера (-d – для удаления привязки к текущей bash сессии, чтобы при ее закрытии контейнер остался запущенным)
|docker stop <имя>	| Остановка запущенного контейнера (сигнал – SIGTERM, если не останавливается - SIGKILL)
|docker start <имя>	| Запуск остановленного контейнера
|docker restart <имя> |	
|docker pause <имя>	| (сигнал – SIGSTOP)
|docker unpause <имя> | 	
|docker kill <имя>	| (сигнал – SIGKILL)
|docker kill –signal=1 <имя> | 	
|docker container prune	| Удаление всех остановленных контейнеров
|docker rename <old_name> <new_name> | 	


