## О проекте  
В рамках спринта Deployhub была реализована инфраструктура для двух приложений — Trekker и Pawster. Основная цель проекта — автоматизировать развёртывание, обеспечить SSL-шифрование и настроить обратное проксирование трафика через Nginx на процессы Gunicorn для API и статические файлы фронтенда. 

## Структура репозитория  
- **infra/** — конфигурации Nginx и systemd для развёртывания приложений
- **backend/** — исходники Django-API для Kittygram и Taski-Docker
- **frontend/** — сборки React-приложений для обеих проектов
- **requirements.txt** — Python-зависимости  
- **pytest.ini** — конфигурация тестирования  

## Стек технологий  
- **OS:** Ubuntu 20.04  
- **Web-сервер:** Nginx (конфиги в `infra/default`)  
- **Application Server:** Gunicorn + systemd (unit-файл `infra/gunicorn_kittygram.service`)  
- **SSL:** Certbot / Let’s Encrypt
- **Backend:** Python, Django, Django REST Framework  
- **Frontend:** React, Redux, Docker (для локальной сборки фронтенда)  

## Быстрое развёртывание

```bash
# 1. Подготовить VM (Ubuntu 20.04), установить Python3, pip, Node.js и Docker
sudo apt update && sudo apt install -y python3-pip python3-venv nginx certbot docker.io

# 2. Клонировать репозиторий и установить зависимости
git clone https://github.com/your-username/infra_sprint1
cd /infra_sprint1
pip3 install -r requirements.txt

# 3. Настроить systemd-unit для Gunicorn
sudo cp infra/gunicorn_kittygram.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now gunicorn_kittygram

# 4. Настроить конфиги Nginx и SSL
sudo cp infra/default /etc/nginx/sites-available/infra_sprint1
sudo ln -s /etc/nginx/sites-available/infra_sprint1 /etc/nginx/sites-enabled/
sudo certbot --nginx -d t4skiii.ddns.net -d kitygram.zapto.org

# 5. Перезапустить Nginx
sudo systemctl restart nginx
