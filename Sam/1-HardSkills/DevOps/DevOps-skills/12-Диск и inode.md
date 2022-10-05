**inode** - данные о файлах и директориях, методанные

Просмотр свободного места на диске
```bash
df -hT
```

Просмотр свободых inode
```bash
df -hTi
```

Удаление открытых файлов
```bash
rm -rf /mnt/junkdirectory/
```

### Logrotate
**Logrotate** - удаление файлов периодически.
```bash
/var/log/nginx/*.log.*.gz {
        daily
        missingok
        rotate 14
        compress
        delaycompress
        notifempty
        create 0640 nginx nginx
        sharedscripts
        prerotate
                if [ -d /etc/logrotate.d/httpd-prerotate ]; then \
                        run-parts /etc/logrotate.d/httpd-prerotate; \
                fi \
        endscript
        postrotate
                invoke-rc.d nginx rotate >/dev/null 2>&1
        endscript
}
```
