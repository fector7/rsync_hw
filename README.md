# Домашнее задание к занятию 3 «Резервное копирование»


### Задание 1
- Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию `/tmp/backup`
- Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
- Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
- На проверку направить скриншот с командой и результатом ее выполнения

![alt text](https://github.com/fector7/rsync_hw/blob/main/img/rsync1.PNG)


### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.

```bash
#!/bin/bash

LOGFILE="/var/log/backup.log"
SOURCE="$HOME"
DEST="/tmp/backup"
EXCLUDE="--exclude='.*/'"

echo "[$(date)] Starting backup..." >> "$LOGFILE"

/usr/bin/rsync -avc --delete $EXCLUDE "$SOURCE" "$DEST" >> "$LOGFILE" 2>&1

if [ $? -eq 0 ]; then
    logger "Backup completed successfully"
    echo "[$(date)] Backup successful" >> "$LOGFILE"
else
    logger "Backup FAILED"
    echo "[$(date)] Backup FAILED" >> "$LOGFILE"
fi
```
cronrab
```
0 2 * * * /home/fedor/backup.sh
```

![alt text](https://github.com/fector7/rsync_hw/blob/main/img/rsync2.PNG)

---

## Задание 3*
- Настройте ограничение на используемую пропускную способность rsync до 1 Мбит/c
- Проверьте настройку, синхронизируя большой файл между двумя серверами
- На проверку направьте команду и результат ее выполнения в виде скриншота

```
rsync --bwlimit=1024 -av --progress largefile fector@172.31.227.71:/tmp/backup/
```
![alt text](https://github.com/fector7/rsync_hw/blob/main/img/rsync3.PNG)