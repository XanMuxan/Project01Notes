# Развертывание Ollama через Docker?
### Термины

- **Ollama** — это инструмент, который позволяет запускать локальные LLM (как LLaMA, Mistral, Gemma и другие)

- **LLaMA** (Large Language Model Meta AI) — это серия открытых языковых моделей, разработанных компанией Meta

- **OpenWebUI** — это веб-интерфейс для Ollama, похожий на ChatGPT, но работающий с локальными моделями

### Требования к ресурсам

Memory 8+ GB  
CPU 4+ (лучше GPU)  
HDD 25+ GB

## Установка Ollama

### Есть два способа:

1. Написать в PowerShell: ``irm https://ollama.com/install.ps1 | iex
2. Скачать на официальном сайте: https://ollama.com/download/windows

## Установка Docker Engiene
[https://docs.docker.com/engine/install/ubuntu](https://docs.docker.com/engine/install/ubuntu)

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
# Install Docker
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### Запуск локальной LLM

Создать файл `entrypoint.sh` и выбрать любую открытую LLM модель
```
#!/bin/bash

/bin/ollama serve &
pid=$!
sleep 5
ollama pull llama3
wait $pid
```
Создать файл Docker Compose манифеста `docker-compose.yaml`
```
---
version: '3.8'
services:
  ollama:
    image: ollama/ollama
    ports:
      - "11434:11434"
    volumes:
      - ./ollama:/root/.ollama
      - ./entrypoint.sh:/entrypoint.sh
    environment:
      - OLLAMA_DEVICE=cpu
    restart: always
    tty: true
    pull_policy: always
    container_name: ollama
    entrypoint: ["/usr/bin/bash", "/entrypoint.sh"]

  openwebui:
    image: ghcr.io/open-webui/open-webui:main
    ports:
      - "80:8080"
    environment:
      - OLLAMA_BASE_URL=http://ollama:11434
    depends_on:
      - ollama
    restart: always
    container_name: openwebui
...
```
Запуск:
```
docker compose up -d
```
Web UI доступен по http://<host_IP>

## Запуск простейшего API запроса
```
from langchain_ollama import ChatOllama

llm = ChatOllama(
    model="llama3:latest",
    base_url="http://<VM_IP>:11434",
)

response = llm.invoke([("human", "What is LLM?")])

print(response.content)
```
# Развертывание через LM Studio

LM Studio - это простой способ запустить нейросети (Llama 3, Mistal, Gemma) локально. Программа предоставляет графический интерфейс для поиска, скачивания и использования моделей в формате GGUF, поддерживает GPU-ускорение (NVIDIA/Apple Metal) и локальный сервер API, работающий без интернета.

## Пошаговая инструкция
### Установка:
1. Скачайте и установите LM Studio с официального сайта.
2. Приложение доступно для Windows, macOS и Linux.
### Поиск и загрузка модели:
1. Откройте вкладку Model search
2. Введите название доступной модели
3. Справа выберите версию модели (квантование Q4_K_M или Q5_K_M — лучший баланс качества и скорости) и нажмите «Download».
### Запуск модели:
1. Перейдите во вкладку Chat
2. Нажмите кнопку New chat
3. Выберите модель в Pick a model
4. Настройте дополнительные параметры, если требуется
### Использование:
1. Вводите запросы в чат
## Ключевые особенности LM Studio
1. Локальный API-сервер: Во вкладке «Developer» можно запустить сервер, совместимый с OpenAI API, что позволяет интегрировать локальные LLM в свои приложения, проекты на Python или использовать с AnythingLLM.
2. Работа в офлайне: Все данные остаются на вашем компьютере.

Для комфортной работы рекомендуется иметь не менее 16 ГБ оперативной памяти (для моделей 7B/8B) и видеокарту NVIDIA (для ускорения).
# Взаимодействие с обоими через OpenWebUI

LM studio на данный момент не имеет официальной поддержки OpenWebUI.s

Об Ollama написано выше.