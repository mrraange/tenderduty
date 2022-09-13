# tenderduty

# Подготавливаем сервер
## обновляем репозитории

```
sudo apt update && sudo apt upgrade -y
```

## устанавливаем необходимые утилиты
 
 ```
sudo apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

## ставим docker 

```
apt update && \
apt install apt-transport-https ca-certificates curl software-properties-common -y && \
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && \
apt update && \
apt-cache policy docker-ce && \
sudo apt install docker-ce -y && \
docker --version
```

## ставим tenderduty

```
tmux new-session -s tenderduty

mkdir tenderduty && cd tenderduty
docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml
```

## качаем конфиг с русским языком и редакрируем его

```
wget -O $HOME/tenderduty/config.yml "https://raw.githubusercontent.com/mrraange/tenderduty/main/config.yml"
nano $HOME/tenderduty/config.yml
```

### Для обычного мониторинга без оповещений достаточно изменить в конфиге:

имя сети:

chain-id:

valoper_address:

url:

### после настройки конфига запускаем

```
docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest
```

### смотрим логи

```
docker logs -f --tail 20 tenderduty
```
![Screenshot_16](https://user-images.githubusercontent.com/100018176/189885054-256590ca-8cc1-43e1-a480-a07b0d6a100e.png)


### проверяем в браузере
```
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8888/\033[0m"
```

пример  http://10.10.10.10:8888/

![photo1663067731](https://user-images.githubusercontent.com/100018176/189887514-9567e023-de34-49d9-84cb-874ae69c8341.jpeg)

# Настройка Telegram

Нам будет необходимо создать своего бота и узнать ID своего telegram или ID необходимой нам группы в telegram

### Создаем своего бота. 

Для этого пишем боту @BotFather  и вводим команду /newbot - далее вводим имя бота - далее username (должно заканчиваться на bot). Бот выдаст token API, который надежно сохраняем и никому не показываем 

![Screenshot_20](https://user-images.githubusercontent.com/100018176/189889538-04c2579a-6cb1-4da3-8755-fe6b525c4d6d.png)

### Узнаем свой ID или ID группы (для этого добавляем бота в группу). Для того, чтобы узнать свой ID пишем боту @JsonViewBot и отправляем ему любое сообщение 

![photo1663068674](https://user-images.githubusercontent.com/100018176/189890268-345064b8-ba20-406c-9ff7-2b227149537c.jpeg)

###  добавляем ID и token API в наш конфиг, после чего перезагружаем мониторинг
```
docker restart tenderduty
```
![Screenshot_22](https://user-images.githubusercontent.com/100018176/189891041-3fb8412c-f9f4-4166-881c-6013d4df4719.png)

# Полезное

### посмотреть установленные образы
```
docker images
```
### посмотреть запущенные контейнеры
```
docker ps
```
### остановить контейнер
```
docker stop tenderduty
```
### перезапустить контейнер
```
docker restart tenderduty
```
### посмотреть логи
```
docker logs -f --tail 20 tenderduty
```
