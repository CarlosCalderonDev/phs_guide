# GUIDE_DEVOPS

---

## ACCEDER LOGS DEL SERVIDOR

#### ACCEDER LOGS DEL SERVIDOR SAAS 1

```bash
ssh root@159.89.190.153 "tail -f /var/log/php-fpm/main-error.log"

```

#### ACCEDER LOGS DEL SERVIDOR SAAS 2

```bash
ssh root@142.93.64.142 "tail -f /var/log/php-fpm/main-error.log"

```
#### ACCEDER LOGS DEL SERVIDOR SAAS 4

```bash
ssh root@167.71.111.172 "tail -f /var/log/php-fpm/main-error.log"

```

---

## SINCRONIZAR SERVIDORES

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

# AYUDAS PARA OBTENER CONTEXTOS

#### QUE VERSION ESTOY USANDO ACTUALMENTE

```bash
php -v

php symfony -V

```

#### Filtrar todas las funciones de una clase

```bash
# Primero me ubico dentro la carpeta que contiene la clase
cd /home/phs/pms-rafael/demo/pacific-sch/lib/model/doctrine

# Segundo: Ingreso el comando
grep -n "function " SchSlotAssigned.class.php
```