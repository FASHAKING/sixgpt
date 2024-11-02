
# SixGPT Miner
This is the official SixGPT miner.

# About

SixGPT is a decentralized synthetic data generation platform built on the Vana network. We empower users by giving them ownership of their data and enabling them to earn from it.

The SixGPT Miner is a software package which allows users to contribute data they generate for wikipedia question-answer based tasks to the network for rewards.
In the future, we will support other tasks such as chatbot conversations, image captioning, etc.

## Prerequisites
(1) install docker on your machine. (https://docs.docker.com/engine/install/)

(2) Fund your wallet with at least 0.1 $VANA

(3) Login with this wallet on sixgpt.xyz


# Run miner

## Before start:
* You must have logged into https://sixgpt.xyz with your `wallet` & `email` before running the miner
* Make sure the wallet associated with your vana private key has enough $VANA balance on the desired network (at least 0.1) - [VANA faucet](https://faucet.vana.org/satori)

## Install Docker
```console
# Docker
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo chmod +x /usr/local/bin/docker-compose

# Docker version check
docker --version
```

## Install sixgpt
**1. Create folders**
```console
mkdir sixgpt
```
```console
cd sixgpt
```

**2. Set Variables - Replace `your_private_key`**
```console
export VANA_PRIVATE_KEY=your_private_key
export VANA_NETWORK=moksha
```

**3. Create docker-compose.yml**
```console
nano docker-compose.yml
```
Enter the following code in it
```console
version: '3.8'

services:
  ollama:
    image: ollama/ollama:0.3.12
    ports:
      - "11435:11434"
    volumes:
      - ollama:/root/.ollama
    restart: unless-stopped
 
  sixgpt3:
    image: sixgpt/miner:latest
    ports:
      - "3015:3000"
    depends_on:
      - ollama
    environment:
      - VANA_PRIVATE_KEY=${VANA_PRIVATE_KEY}
      - VANA_NETWORK=${VANA_NETWORK}
    restart: always

volumes:
  ollama:
```
To save: `CTRL+X+Y`+`ENTER`

 Set Variables - Replace `your_private_key`**
```console
export VANA_PRIVATE_KEY=your_private_key
export VANA_NETWORK=moksha
```

## Start miner
```
docker compose up -d
```

## Logs
```
docker compose logs -fn 100
```

