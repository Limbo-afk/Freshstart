# Freshstart
## Описание задачи
<br>Задача:
<br>![380424642-6ccebb43-249b-4d0a-b1e7-5bb8dab805e1](https://github.com/user-attachments/assets/5e4c8f1d-8e96-485a-b57b-6ff2a452693a)
## Начало
<br>Накатить образ Linux Выделить 2+ ядер. Выделать от 4096+ МБ оперативы. При установки операционной системы, выбрать предпочтительный язык системы.
<br>Устанавлием утилиту wget, используя менеджер пакетов yum
```
sudo yum install wget
```
<br>![1](https://github.com/user-attachments/assets/c4c213ae-141f-4307-9907-9cb1c771614c)

Скачиваем файл с указанного репозитория и сохраняем его в директорию /etc/yum.repos.d/. Этот файл нужен для получения информации откуда выгружаются пакеты Docker
```
sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
```
<br>![2](https://github.com/user-attachments/assets/4081d5f2-7109-40c4-80a3-c8a7a2d72cca)
<br>Устанавлием непосредтсвенно пакеты Docker (сам Docker Engine, Docker CLI и Containerd).
+ `docker-ce` — это основной пакет Docker Community Edition.
+ `docker-ce-cli` — это командная строка Docker, которая позволяет взаимодействовать с Docker через терминал.
+ `containerd.io` — это контейнерный демон, который управляет жизненным циклом контейнеров.

```
sudo yum install docker-ce docker-ce-cli containerd.io
```
<br>![3](https://github.com/user-attachments/assets/6a739009-4e89-428f-87a6-10ea6780a39a)

<br> Подтверждаем

<br>![4](https://github.com/user-attachments/assets/f5520071-edbb-4fb2-aa9e-5091cb43d4a9)

<br>Автозагрузка Docker
```
sudo systemctl enable docker --now
```
<br>![5](https://github.com/user-attachments/assets/4a8bc27d-42e1-47d2-8c6f-678feea77193)

<br>Установка curl
```
sudo yum install curl
```
<br>![6](https://github.com/user-attachments/assets/7f2d289d-7eee-4f71-8e12-29bfbc0fb900)

<br>Обращаемся к репозиторию, чтобы получить последнюю версию Docker Compose.
<br>Обратите внимание(!), что команду нужно вводить без sudo, потому что она только получает информацию о версии и не требует прав суперпользователя для выполнения.
```
COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
```

<br>Загружаем исполняемый файл Docker Compose соответствующей версии для вашей системы.
```
sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
```

<br>![7](https://github.com/user-attachments/assets/195cf68e-47f0-4c72-9886-5720d41dc0e8)

<br>Изменяем права доступа для файла (права на выполнение как исполняемого файла).
```
sudo chmod +x /usr/bin/docker-compose
```

<br>Показывает версию Docker Compose.
```
docker-compose --version
```
<br>![8](https://github.com/user-attachments/assets/4ed2a4bd-a511-4240-b781-dec25cdbcc0a)

