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
Скачиваем файл с указанного репозитория и сохраняем его в директорию /etc/yum.repos.d/. Этот файл нужен для получения информации откуда выгружаются пакеты Docker
```
sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
```
