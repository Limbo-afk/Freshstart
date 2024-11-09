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
## Git and Grafana

<br>Установка гита + клонирование репозитория
```
git clone https://github.com/skl256/grafana_stack_for_docker.git
```
<br>![image](https://github.com/user-attachments/assets/d330fe56-f58c-483b-b5ba-080d877dad26)

<br>Переходим в папку `grafana_stack_for_docker`
```
cd grafana_stack_for_docker
```
<br>![10](https://github.com/user-attachments/assets/76ad8975-4ee6-4af1-af10-0c81dbcf882e)

<br>Создаем директоррию `config`
```
sudo mkdir -p /mnt/common_volume/swarm/grafana/config
```

<br>Создаем несколько диреткорий для хранения конфигурационных файлов и данных от Grafana.
```
sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data}
```

<br>Меняет владельца на того, кто выполнил команду (для управления).
```
sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}
```

<br>Если файла нет - создает, если есть изменяет временные метки доступа до текущего времени, не изменяя содержимое.
```
touch /mnt/common_volume/grafana/grafana-config/grafana.ini
```

<br>Копируем **все файлы** из диреткории `config` в директорию `/mnt/common_volume/swarm/grafana/config/`.
```
cp config/* /mnt/common_volume/swarm/grafana/config/
```

<br>Переименовываем файл `grafana.yaml` в `docker-compose.yaml`. (можно проверить через ls)
```
mv grafana.yaml docker-compose.yaml
```
<br>![11](https://github.com/user-attachments/assets/369dff69-261a-4209-bf3a-fe618ba95ce9)

<br>Поднимаем Docker Compose.
```
sudo docker compose up -d
```
<br>![12](https://github.com/user-attachments/assets/da5db25d-174d-4937-8a86-2ca81d2a1ba7)
<br>![13](https://github.com/user-attachments/assets/a030d55e-cadd-455a-bf64-343311957980)
## Работа в Grafana
<br>Находится на `localhost:3000`
<br>Данные для авторизации:
<br>Username: admin
<br>Password: admin
<br>![14](https://github.com/user-attachments/assets/1f1a192c-fda8-4cc0-b4c0-345f02855500)

<br>Тут для того чтобы брать откуда-то данные вставляем экспортер.
```
cd grafana_stack_for_docker
```
```
sudo vi docker-compose.yaml
```
```node-exporter:
    image: prom/node-exporter
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    container_name: exporter
    hostname: exporter
    command:
      - --path.procfs=/host/proc
      - --path.sysfs=/host/sys
      - --collector.filesystem.ignored-mount-points
      - ^/(sys|proc|dev|host|etc|rootfs/var/lib/docker/containers|rootfs/var/lib/docker/overlay2|rootfs/run/docker/netns|rootfs/var/lib/docker/aufs)($$|/)
    ports:
      - 9100:9100
    restart: unless-stopped
    environment:
      TZ: "Europe/Moscow"
    networks:
      - default
```
<br>А затем в `/mnt/common_volume/swarm/grafana/config/prometheus.yaml` меняем targets:(тут айпишник) на **exporter:9100**.
<br> ![15](https://github.com/user-attachments/assets/74737eae-2b47-4624-81d6-3574f35548fb)
<br> АХАХАХАХАХАХАХА ОНО РАБОТАЕТ!!!!!!
<br>![16](https://github.com/user-attachments/assets/c6b59c62-ba99-497e-bb6a-7bfd1735909a)

## VictoriaMetrics
<br>Останавливаем докер.
```
sudo docker-compose stop
```
<br>Переделываем `docker-compose.yaml`. Убираем все лишнее, а ниже код к VictoriaMetrics, который надо **добавить**.
```
 victoriametrics:
    container_name: victoriametrics
    image: victoriametrics/victoria-metrics:v1.105.0
    ports:
      - 8428:8428
      - 8089:8089
      - 8089:8089/udp
      - 2003:2003
      - 2003:2003/udp
      - 4242:4242
    volumes:
      - vmdata:/storage
    command:
      - "--storageDataPath=/storage"
      - "--graphiteListenAddr=:2003"
      - "--opentsdbListenAddr=:4242"
      - "--httpListenAddr=:8428"
      - "--influxListenAddr=:8089"
      - "--vmalert.proxyURL=http://vmalert:8880"
    networks:
      - vm_net
    restart: always
```
<br>Снова поднимаем
```
sudo docker compose up -d
```
<br>Команда отправляет метрику OILCOINT_metric2 с типом gauge и значением 0 на локальный сервер Prometheus по адресу http://localhost:8428/api/v1/import/prometheus. Она использует echo для формирования данных и curl для их передачи.
```
echo -e "# TYPE OILCOINT_metric2 gauge\nOILCOINT_metric2 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus
```
<br>Команда выполняет GET-запрос к локальному серверу Prometheus для получения значения метрики OILCOINT_metric2.
```
curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric2'
```
<br>Повторяем команду, но со значением 50, чтобы увидеть изменения.
```
echo -e "# TYPE OILCOINT_metric2 gauge\nOILCOINT_metric2 50" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus
```
<br>Открываем VictoriaMetrics на `localhost:8428`
<br>![18](https://github.com/user-attachments/assets/4ce68e7c-2e7a-4eff-b3a8-b603a0d1dc94)
<br>Переходим в `vmui - Web UI` и видим отображение наших запросов, значит данные доходят.
<br>![17](https://github.com/user-attachments/assets/92bbf304-5cf5-49ca-910e-d681e36f5940)
<br>Добавляем Викторию Dashboard и КОНЕЦ!
<br>![19](https://github.com/user-attachments/assets/1e99c26d-5946-4326-8f60-54dc24f9c752)

