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
wget -O $HOME/tenderduty/config.yml "https://raw.githubusercontent.com/lesnikutsa/lesnik_utsa/main/monitoring/TenderDuty(ru)/config.yml"
nano $HOME/tenderduty/config.yml
```
