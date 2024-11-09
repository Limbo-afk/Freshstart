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
