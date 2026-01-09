# Практическая работа: Дополнительные возможности Docker

## Цель работы

Изучение дополнительных возможностей Docker для управления жизненным циклом контейнеров, мониторинга их состояния и оптимизации использования системных ресурсов. Данная практика направлена на глубокое понимание операционных аспектов контейнеризации, что критически важно для построения надежных и масштабируемых приложений в production-среде.

---

## Ход работы

### Подготовка окружения

```bash
# Создание директории для практики
mkdir docker-practice
cd docker-practice

# Копирование Dockerfile и entrypoint.sh в директорию
# (файлы должны быть созданы согласно примерам выше)

# Сборка образа
docker build -t practice-image:1.0 .
```
<img width="974" height="628" alt="image" src="https://github.com/user-attachments/assets/9ff88b6e-9444-4e24-ae1e-50ddb26611e6" />


### Задание 1: Вывод логов в файл

```bash
# Запуск контейнера
docker run -d --name practice-container-1 practice-image:1.0 30

# Ожидание завершения контейнера
sleep 35

# Сохранение логов
docker logs practice-container-1 > /tmp/container_logs.txt

# Просмотр логов
cat /tmp/container_logs.txt

# Очистка
docker rm practice-container-1
```
<img width="974" height="52" alt="image" src="https://github.com/user-attachments/assets/5b32a0f7-4c23-43de-9434-5936de8b6b4a" />


<img width="974" height="551" alt="image" src="https://github.com/user-attachments/assets/84248b8a-5850-4543-9882-6c058d4436ac" />



### Задание 2: Проверка docker-stats

```bash
# Запуск контейнера в background
docker run -d --name practice-container-2 practice-image:1.0 45

# В другом терминале: просмотр статистики в реальном времени
docker stats practice-container-2

# Или сохранение одного снимка статистики
docker stats --no-stream practice-container-2 > /tmp/container_stats.txt

# После завершения контейнера
docker rm practice-container-2
```
<img width="974" height="422" alt="image" src="https://github.com/user-attachments/assets/2778df0d-5ad0-4160-859c-0156c902e0e0" />


<img width="974" height="197" alt="image" src="https://github.com/user-attachments/assets/ef2f4e07-52d1-44b9-97cd-47a280aeb2a6" />


### Задание 3: Ограничение ресурсов

```bash
# Запуск контейнера с ограничением памяти и CPU
docker run -d --name practice-limited \
  --memory=256m \
  --cpus=0.5 \
  practice-image:1.0 60

# Проверка статистики с учетом лимитов
docker stats --no-stream practice-limited

# Обновление лимитов на работающем контейнере
docker update --memory=512m practice-limited

# Очистка
docker stop practice-limited
docker rm practice-limited
```
<img width="974" height="328" alt="image" src="https://github.com/user-attachments/assets/1cc95d6d-ebc3-4807-9dae-3fd45248d390" />


### Задание 4: Экспорт в tar

```bash
# Запуск контейнера и ожидание его завершения
docker run -d --name practice-export practice-image:1.0 30

# Ожидание завершения
sleep 35

# Экспорт контейнера в tar
docker export practice-export > /tmp/container_export.tar

# Проверка размера архива
ls -lh /tmp/container_export.tar

# Просмотр содержимого архива
tar -tf /tmp/container_export.tar | head -20

# Очистка
docker rm practice-export
```

<img width="974" height="537" alt="image" src="https://github.com/user-attachments/assets/29378603-096a-4425-b342-a105d4eba808" />


### Задание 5: Импорт из tar

```bash
# Загрузка образа из архива
docker import /tmp/container_export.tar restored-practice:1.0

# Проверка наличия образа
docker images | grep restored

# Запуск контейнера из загруженного образа
docker run -d --name restored-from-tar restored-practice:1.0 /home/appuser/entrypoint.sh

# Проверка логов восстановленного контейнера
sleep 35
docker logs restored-from-tar

# Очистка
docker stop restored-from-tar
docker rm restored-from-tar
docker rmi restored-practice:1.0
```
<img width="974" height="351" alt="image" src="https://github.com/user-attachments/assets/76851cc4-8fe8-4470-abae-68c4d58ffeae" />


<img width="974" height="842" alt="image" src="https://github.com/user-attachments/assets/2b65d2df-4f91-49a6-89b4-d74a744e9f50" />


---

## Вывод

В ходе выполнения практической работы были изучены дополнительные возможности Docker по управлению жизненным циклом контейнеров, мониторингу их состояния и контролю использования системных ресурсов. Были получены практические навыки работы с логами контейнеров, инструментом docker stats, а также механизмами ограничения ресурсов по CPU и памяти. Кроме того, освоены операции экспорта и импорта контейнеров, позволяющие сохранять и восстанавливать состояние приложений. Полученные знания и навыки обеспечивают понимание операционных аспектов контейнеризации, необходимых для построения надёжных и масштабируемых приложений в production-среде.
