# PHS_GUIDE

#### SUMMARY

```bash
cd home/phs/pms-rafael/ # CLIENTE
cd home/phs/pms-rafael/demo/ # AMBIENTE
cd home/phs/pms-rafael/demo/pacific-sch/ # MODULO
```

#### ACCEDER LOGS DEL SERVIDOR SAAS 1

```bash
clear

ssh root@159.89.190.153 "tail -f /var/log/php-fpm/main-error.log"

```

#### ACCEDER LOGS DEL SERVIDOR SAAS 2

```bash
clear

ssh root@142.93.64.142 "tail -f /var/log/php-fpm/main-error.log"

```

#### ACCEDER LOGS DEL SERVIDOR SAAS 3

```bash
clear

ssh root@45.55.49.177 "tail -f /var/log/php-fpm/main-error.log"

```

#### ACCEDER LOGS DEL SERVIDOR SAAS 4

```bash
clear

ssh root@167.71.111.172 "tail -f /var/log/php-fpm/main-error.log"

```

#### SINCRONIZAR SERVIDORES 1

```bash
cd /home/phs/pms-rafael/demo/pacific-com

cd ../pms-scripts/pms-install/ && ./pms-rsync -p pacific-com

```

#### SINCRONIZAR SERVIDORES 2

```bash
cd /home/phs/pms-rafael/demo/pacific-com

cd ../pms-scripts/pms-install/ && ./pms-rsync -p pacific-com

./pms-rsync -p pacific-com


```
