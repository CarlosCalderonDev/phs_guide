# MI_GUIDE_APP_FOR_DEVOPS

#### ACCEDER LOGS DEL SERVIDOR

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

#### SINCRONIZAR SERVIDORES

```bash
# UBICACIONES
cd home/phs/pms-rafael/ # CLIENTE
cd home/phs/pms-rafael/demo/ # AMBIENTE
cd home/phs/pms-rafael/demo/pacific-sch/ # MODULO
# POSICIONARME SIEMPRE DENTRO DEL AMBIENTE PARA PODER SINCRONIZAR
/home/phs/pms-rafael/demo/pacific-ehd/apps/ehd/modules/desktop/actions/actions.class.php
4272
# UBICARME PRIMERO, PRIMER SINCRONIZADO, SINCRONIZADO EN ADELANTE
cd /home/phs/pms-rafael/demo/pacific-com
cd ../pms-scripts/pms-install/ && ./pms-rsync -p pacific-com
./pms-rsync -p pacific-com
```

#### VERIFICADORES

```php
#TESTING USE EXECUTE
$this->getResponse()->setHttpHeader("X-Debug-Execute", '{"X-USE_THIS_AJAX_HERE!"}');
#TESTING USE JAVASCRIPT FRONTEND
console.log(transport.getResponseHeader("X-Debug-Execute", 'X-USE_THIS_AJAX_HERE!'));
```

#### Filtrar todas las funciones de uan clase

```bash
# Primero me ubico dentro la carpeta que contiene la clase
cd /home/phs/pms-rafael/demo/pacific-sch/lib/model/doctrine

# Segundo: Ingreso el comando
grep -n "function " SchSlotAssigned.class.php
```

#### QUE VERSION ESTOY USANDO ACTUALMENTE

```bash
php -v
php symfony -V
```

# Ireport

```bash
sudo ./Descargas/iReport-5.6.0/bin/ireport
```

# Reiniciar jasper

```bash
/opt/jasperreports-server/./ctlscript.sh restart
/opt/jasperreports-server/./ctlscript.sh status
```

# Oncomedical server

```bash
ssh pacific@192.168.0.7
Fletcher_47*+
sudo tail -f /var/log/php-fpm/www-error.log
```


PARTE1: /home/phs/pms-rafael/demo/pacific-ehd/apps/ehd/modules/component/templates/_compositionOrders.php

<?php else: ?>
  <?php $message = new SystemMessage($culture, $default)?>
  <?php $message->setCustomMessage(__('No hay ordenes por imprimir para esta atención'), 'info')?>
  <script>
    <?php echo $message->getHtml(false) ?>
    jQuery('.modal:last').modal('hide');
  </script>
<?php endif;?>

```bash
showMessenger("Se elimino el servicio", "success");
```


