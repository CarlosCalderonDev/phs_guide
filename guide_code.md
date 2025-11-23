# MI_GUIDE_APP_FOR_SYMFONY_PHP

#### PASOS PARA UNA NUEVA TABLA

```bash
# PASO 1: CREO EL MODELO EN EL ESQUEMA - UBICATE EN EL MODULO QUE VAS A CAMBIAR (EJEMPLO: /home/phs/pms-rafael/demo/pacific-sch)
/home/phs/pms-rafael/demo/pacific-com/config/doctrine/gbl_schema.yml
# PASO 2: EJECUTAR EL COMANDO (ESTO GENERAL EL MODELO Y EL SQL)
./symfony doctrine:build --model --sql
./symfony doctrine:build --model
./symfony doctrine:build --sql
# PASO 3: UBICARME PRIMERO, PRIMER SINCRONIZADO, SINCRONIZADO EN ADELANTE
cd /home/phs/pms-rafael/demo/pacific-com

cd ../pms-scripts/pms-install/ && ./pms-rsync -p pacific-com

./pms-rsync -p pacific-com

# PASO 4: SQL QUE SE GENERO
# AQUI: /home/phs/pms-rafael/demo/pacific-sch/data/sql/schema.sql) CON ESTO CREAR LA TABLA EN LA BASE DE DATOS
CREATE TABLE gbl_email_log (id BIGSERIAL, gbl_entity_id BIGINT, sch_slot_assigned_id BIGINT, sch_attention_worklist_id BIGINT, prefix_and_seq_number VARCHAR(21), email VARCHAR(255) NOT NULL, alias VARCHAR(255) NOT NULL, json_data text, message_id VARCHAR(255), created_at TIMESTAMP NOT NULL, PRIMARY KEY(id));
ALTER TABLE gbl_email_log ADD CONSTRAINT gssi FOREIGN KEY (sch_attention_worklist_id) REFERENCES sch_attention_worklist(id) ON UPDATE CASCADE ON DELETE RESTRICT NOT DEFERRABLE INITIALLY IMMEDIATE;
ALTER TABLE gbl_email_log ADD CONSTRAINT gbl_email_log_sch_slot_assigned_id_sch_slot_assigned_id FOREIGN KEY (sch_slot_assigned_id) REFERENCES sch_slot_assigned(id) ON UPDATE CASCADE ON DELETE RESTRICT NOT DEFERRABLE INITIALLY IMMEDIATE;
ALTER TABLE gbl_email_log ADD CONSTRAINT gbl_email_log_gbl_entity_id_gbl_entity_id FOREIGN KEY (gbl_entity_id) REFERENCES gbl_entity(id) ON UPDATE CASCADE ON DELETE RESTRICT NOT DEFERRABLE INITIALLY IMMEDIATE;
# PASO 5: DOY PERMISO A LAS TABLAS
GRANT ALL ON SEQUENCE gbl_email_log_id_seq TO "phs-adm", "phs-user", "phs-soap";
GRANT ALL ON TABLE gbl_email_log TO "phs-adm" , "phs-user", "phs-soap";
# PASO 6: MODELO QUE SE GENERADO
# AQUI: /home/phs/pms-rafael/demo/pacific-sch/lib/model/doctrine/GblEmailLog.class.php
```

#### VERIFICADORES

```php
#TESTING USE EXECUTE
$this->getResponse()->setHttpHeader("X-Debug-Execute", '{"X-USE_THIS_AJAX_HERE!"}');
#TESTING USE JAVASCRIPT FRONTEND
console.log(transport.getResponseHeader("X-Debug-Execute", 'X-USE_THIS_AJAX_HERE!'));
```

---

#### TRAZAR & DEBUGUEAR

```php
// ----------------------------------------------------------------------------------------------------
error_log("# ----- ----- ----- ----- ----- # TARGET_HERE! # ----- ----- ----- ----- ----- #"); # CARLOS00110001_TRAZAS_HERE
error_log('MAIL BODY 1 -> ' . var_export($changeThisVar, true));
error_log('MAIL BODY JSON 2 -> ' . json_encode($changeThisVar));
error_log('MAIL BODY (500) 3 -> ' . substr(var_export($changeThisVar, true), 0, 500));
error_log("# ----- ----- ----- ----- ----- # TARGET_HERE! # ----- ----- ----- ----- ----- #"); # CARLOS00110001_TRAZAS_HERE
error_log('TIPO: ' . gettype($changeThisVar));
error_log('TIPO + DETALLE: ' . gettype($changeThisVar) . ' -> ' . json_encode($changeThisVar));
error_log('DEBUG: ' . var_export($changeThisVar, true));
error_log("# ----- ----- ----- ----- ----- # TARGET_HERE! # ----- ----- ----- ----- ----- #"); # CARLOS00110001_TRAZAS_HERE
error_log('COMPROBAR QUE ES O QUE TRAEN EN CONSOLA -> ' . json_encode([
    'type'   => $type,
    'prefix' => $prefix,
    'number' => $number,
]));

// ----------------------------------------------------------------------------------------------------
```

#### DEBUGIAR

```bash
tail -f /var/log/php-fpm/main-error.log
#CARLOS00110001_TRAZAS_HERE
<!--CARLOS00110001_TRAZAS_HERE-->
error_log("_______________________________________________________PART"); #CARLOS00110001_TRAZAS_HERE
error_log("CARLOS00110001_TRAZA_HERE: id=" . json_encode($param)); # CARLOS00110001_TRAZAS_HERE
```

```bash
error_log(print_r($request->getParameterHolder()->getAll(), true));
```

```php
error_log("__________________________________________________");
error_log('Contenido completo de response_formatted:');
error_log(print_r($response_formatted, true));   // imprime todo el objeto/arreglo
error_log("__________________________________________________");
```

#### INSPECCIONAR ERRORES

```php
try {
   // código
} catch (Exception $e) {
   error_log("ERROR: " . $e->getMessage());
}
```

#### HELPERS

```bash
# FOR HELPERS
/home/phs/pms-rafael/demo/pacific-urg/plugins/phsCorePlugin/lib/helper/PacificHelper.php
```

# Consulta para traer las tablas que tienen esa columna 

```bash
select table_name from information_schema.columns where column_name = 'gbl_entity_id'
```

# DEMO

/home/phs/pms-rafael/demo/pacific-com/config/doctrine/gbl_schema.yml
./symfony doctrine:build --model --sql

/home/phs/pms-rafael/demo/pacific-sch/data/sql/schema.sql

cd /home/phs/pms-rafael/demo/pacific-com
cd ../pms-scripts/pms-install/ && ./pms-rsync -p pacific-com
./pms-rsync -p pacific-com

/home/phs/pms-rafael/demo/pacific-com/lib/model/doctrine/GblEmailLog.class.php
/home/phs/pms-rafael/demo/pacific-com/lib/ComEmailSender.php
/home/phs/pms-rafael/demo/pacific-com/lib/api/PMSDian.class.php
/home/phs/pms-rafael/demo/pacific-com/apps/pos/modules/paypoint/actions/actions.class.php

# PRODUCCION




# Facturación electrónica: Documentos emitidos
/home/phs/pms-rafael/pacific-com/apps/pos/modules/paypoint/actions/actions.class.php

executeElectronicBilling
   executeGetDocumentMailLog
      getDocumentMailLog # Comprobante No. FELE-168503 /home/phs/pms-rafael/pacific-com/apps/pos/modules/paypoint/actions/actions.class.php


