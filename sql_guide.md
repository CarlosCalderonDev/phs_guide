# MY GUIDE SQL

#### comandos para inspeccionar tablas

```sql
-- ver el SQL con el que fue creada (version 1)
SELECT pg_get_tabledef('com_invoice_document'::regclass);
-- ver el SQL con el que fue creada (version 2)
SELECT pg_get_tabledef(oid)
FROM pg_class
WHERE relname = 'com_invoice_document';
-- ver el SQL con el que fue creada (version 3)
\d com_invoice_document
-- ver el SQL con el que fue creada (version 4)
SELECT
    'CREATE TABLE ' || relname || E'\n(\n' ||
    array_to_string(
        array_agg(
            '    ' || column_name || ' ' || data_type ||
            CASE WHEN character_maximum_length IS NOT NULL
                 THEN '(' || character_maximum_length || ')'
            ELSE '' END
        ), E',\n'
    ) || E'\n);'
FROM information_schema.columns
JOIN pg_class ON table_name = relname
WHERE table_name = 'com_invoice_document'
GROUP BY relname;
```

#### comandos para inspeccionar tablas

# LOGICA DE NEGOCIO

```sql
-- (PostgreSQL) db0 (10.132.14.134) demo
-- (PostgreSQL) db1 (10.132.236.34)
-- (PostgreSQL) db2 (10.132.230.185)
-- (PostgreSQL) db3 (10.132.236.35)
-- (PostgreSQL) oncomed (190.145.36.126)
-- (PostgreSQL) db1-cem (104.236.194.93)
-- (PostgreSQL) san rafael (45.55.64.192)
```

sf prod db1


```sql
SELECT prefix, seq_number, COUNT(*) AS total
FROM com_invoice_document
GROUP BY prefix, seq_number
HAVING COUNT(*) > 1;
```

```sql
SELECT
    tc.table_name,
    kcu.column_name,
    ccu.table_name AS foreign_table,
    ccu.column_name AS foreign_column
FROM information_schema.table_constraints tc
JOIN information_schema.key_column_usage kcu
     ON tc.constraint_name = kcu.constraint_name
JOIN information_schema.constraint_column_usage ccu
     ON ccu.constraint_name = tc.constraint_name
WHERE tc.constraint_type = 'FOREIGN KEY'
  AND tc.table_name = 'com_invoice_document';
```

# un simple sql de ejemplo

```sql
SELECT *
FROM com_invoice_document
WHERE prefix = 'FELE' AND seq_number = 14460;
```

# extrae las llaves foraneas de una tablas

```sql
-- Si devuelve filas → tiene padre(s).
-- Si no devuelve nada → es tabla raíz (padre).
SELECT
    tc.table_name,
    kcu.column_name,
    ccu.table_name AS foreign_table,
    ccu.column_name AS foreign_column
FROM information_schema.table_constraints tc
JOIN information_schema.key_column_usage kcu
     ON tc.constraint_name = kcu.constraint_name
JOIN information_schema.constraint_column_usage ccu
     ON ccu.constraint_name = tc.constraint_name
WHERE tc.constraint_type = 'FOREIGN KEY'
  AND tc.table_name = 'com_invoice_document';
```

# 3. ¿Cómo saber si com_invoice_document es tabla hija o padre?

#### Para saber si es HIJA

```sql
SELECT *
FROM information_schema.table_constraints
WHERE table_name = 'com_invoice_document'
AND constraint_type = 'FOREIGN KEY';
```

#### Para saber si es PADRE

```sql
SELECT
    tc.table_name AS child_table,
    kcu.column_name AS child_column
FROM information_schema.table_constraints tc
JOIN information_schema.key_column_usage kcu
     ON tc.constraint_name = kcu.constraint_name
JOIN information_schema.constraint_column_usage ccu
     ON ccu.constraint_name = tc.constraint_name
WHERE tc.constraint_type = 'FOREIGN KEY'
  AND ccu.table_name = 'com_invoice_document';
```


```sql
DROP TABLE gbl_email_log;
```


```sql
```


```sql
```

FORMA 1


CREATE TABLE gbl_email_log_pmsdian (id BIGSERIAL, third_party_id BIGINT, prefix VARCHAR(10), seq_number BIGINT, alias VARCHAR(255) NOT NULL, email VARCHAR(255) NOT NULL, sent_at TIMESTAMP, status VARCHAR(50), attachments text, raw_payload text, event_count BIGINT DEFAULT 0, created_at TIMESTAMP NOT NULL, updated_at TIMESTAMP NOT NULL, PRIMARY KEY(id));
ALTER TABLE gbl_email_log_pmsdian ADD CONSTRAINT gbl_email_log_pmsdian_third_party_id_gbl_entity_id FOREIGN KEY (third_party_id) REFERENCES gbl_entity(id) ON UPDATE CASCADE ON DELETE RESTRICT NOT DEFERRABLE INITIALLY IMMEDIATE;
GRANT ALL ON SEQUENCE gbl_email_log_pmsdian_id_seq TO "phs-adm", "phs-user", "phs-soap";
GRANT ALL ON TABLE gbl_email_log_pmsdian TO "phs-adm" , "phs-user", "phs-soap";


CREATE TABLE gbl_email_log_pmsdian_event (id BIGSERIAL, gbl_email_log_pmsdian_id BIGINT NOT NULL, sha256 VARCHAR(64) NOT NULL, recipient VARCHAR(255) NOT NULL, type VARCHAR(50) NOT NULL, received_at TIMESTAMP NOT NULL, details text, created_at TIMESTAMP NOT NULL, PRIMARY KEY(id));
ALTER TABLE gbl_email_log_pmsdian_event ADD CONSTRAINT gggi_20 FOREIGN KEY (gbl_email_log_pmsdian_id) REFERENCES gbl_email_log_pmsdian(id) ON UPDATE CASCADE ON DELETE CASCADE NOT DEFERRABLE INITIALLY IMMEDIATE;
CREATE UNIQUE INDEX unique_log_event ON gbl_email_log_pmsdian_event (gbl_email_log_pmsdian_id, sha256);
GRANT ALL ON SEQUENCE gbl_email_log_pmsdian_event_id_seq TO "phs-adm", "phs-user", "phs-soap";
GRANT ALL ON TABLE gbl_email_log_pmsdian_event TO "phs-adm" , "phs-user", "phs-soap";


























INSERT INTO "gbl_email_log_pmsdian" ("id", "third_party_id", "prefix", "seq_number", "alias", "email", "sent_at", "status", "attachments", "raw_payload", "event_count", "created_at", "updated_at") VALUES
(36,	134273,	'FELE',	168756,	'pmsdian_getdocumentmaillog',	'carlos.calderon@pacific-hs.com',	'2025-11-20 21:41:34',	'Sent',	'["z09004479690002500046534.zip"]',	'[{"recipients":["carlos.calderon@pacific-hs.com","carlos.calderon01@outlook.com"],"receivedAt":"2025-11-20T21:41:34-05:00","attachments":["z09004479690002500046534.zip"],"status":"Sent","messageEvents":[{"recipient":"carlos.calderon01@outlook.com","type":"Delivered","receivedAt":"2025-11-20T21:41:36.0000000-05:00","details":"smtp;250 2.6.0 <ae027887-6d7d-4955-870c-5a213050259f@mtasv.net> [InternalId=10286446712210, Hostname=LV4PR05MB11200.namprd05.prod.outlook.com] 64654 bytes in 0.222, 284.250 KB/sec Queued mail for delivery -> 250 2.1.5"},{"recipient":"carlos.calderon@pacific-hs.com","type":"Delivered","receivedAt":"2025-11-20T21:41:35.0000000-05:00","details":"smtp;250 2.0.0 OK 1763692895 d75a77b69052e-4ee48d19aacsi14375621cf.66 - gsmtp"},{"recipient":"carlos.calderon01@outlook.com","type":"Opened","receivedAt":"2025-11-21T02:42:04.0000000Z","details":"Email opened with Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36 from Santiago de Cali, Departamento del Valle del Cauca"},{"recipient":"carlos.calderon@pacific-hs.com","type":"Opened","receivedAt":"2025-11-21T02:41:47.0000000Z","details":"Email opened with Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko Firefox/11.0 (via ggpht.com GoogleImageProxy)"}]},{"recipients":["carlos.calderon@pacific-hs.com"],"receivedAt":"2025-11-20T21:37:19-05:00","attachments":["z09004479690002500046534.zip"],"status":"Sent","messageEvents":[{"recipient":"carlos.calderon@pacific-hs.com","type":"Delivered","receivedAt":"2025-11-20T21:37:20.0000000-05:00","details":"smtp;250 2.0.0 OK 1763692640 d75a77b69052e-4ee48e86435si13293011cf.777 - gsmtp"},{"recipient":"carlos.calderon@pacific-hs.com","type":"Opened","receivedAt":"2025-11-21T02:41:47.0000000Z","details":"Email opened with Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko Firefox/11.0 (via ggpht.com GoogleImageProxy)"}]},{"recipients":["dianita7811@hotmail.com"],"receivedAt":"2025-11-20T17:44:25-05:00","attachments":["z09004479690002500046534.zip"],"status":"Sent","messageEvents":[{"recipient":"dianita7811@hotmail.com","type":"Delivered","receivedAt":"2025-11-20T17:44:40.0000000-05:00","details":"smtp;250 2.6.0 <e9097a54-0b52-4960-bbd7-162aa523f9fe@mtasv.net> [InternalId=122355028344961, Hostname=SN7PR12MB7452.namprd12.prod.outlook.com] 65306 bytes in 0.623, 102.217 KB/sec Queued mail for delivery -> 250 2.1.5"}]}]',	4,	'2025-11-20 21:36:07',	'2025-11-20 21:42:30');
(37,	136991,	'FELE',	168770,	'pmsdian_getdocumentmaillog',	'km120sur2003@yahoo.com',	'2025-11-21 07:16:29',	'Sent',	'["z09004479690002500046542.zip"]',	'[{"recipients":["km120sur2003@yahoo.com"],"receivedAt":"2025-11-21T07:16:29-05:00","attachments":["z09004479690002500046542.zip"],"status":"Sent","messageEvents":[{"recipient":"km120sur2003@yahoo.com","type":"Delivered","receivedAt":"2025-11-21T07:16:30.0000000-05:00","details":"smtp;250 ok dirdel"}]}]',	1,	'2025-11-21 07:32:51',	'2025-11-21 07:32:51'),
(27,	137850,	'FELE',	168754,	'pmsdian_getdocumentmaillog',	'audreydylan2014@hotmail.com',	'2025-11-20 17:04:29',	'Sent',	'["z09004479690002500046525.zip"]',	'[{"recipients":["audreydylan2014@hotmail.com"],"receivedAt":"2025-11-20T17:04:29-05:00","attachments":["z09004479690002500046525.zip"],"status":"Sent","messageEvents":[{"recipient":"audreydylan2014@hotmail.com","type":"Delivered","receivedAt":"2025-11-20T17:04:30.0000000-05:00","details":"smtp;250 2.6.0 <3e5f7165-10e7-4844-aec7-279f652c627d@mtasv.net> [InternalId=27500675598335, Hostname=MW5PR84MB1474.NAMPRD84.PROD.OUTLOOK.COM] 64872 bytes in 0.535, 118.378 KB/sec Queued mail for delivery -> 250 2.1.5"}]}]',	1,	'2025-11-20 21:07:10',	'2025-11-20 21:07:10'),
(23,	137795,	'FELE',	168763,	'pmsdian_getdocumentmaillog',	'luisalbertopanameno@hotmail.com',	'2025-11-20 17:44:25',	'Sent',	'["z09004479690002500046535.zip"]',	'[{"recipients":["luisalbertopanameno@hotmail.com"],"receivedAt":"2025-11-20T17:44:25-05:00","attachments":["z09004479690002500046535.zip"],"status":"Sent","messageEvents":[{"recipient":"luisalbertopanameno@hotmail.com","type":"Delivered","receivedAt":"2025-11-20T17:44:27.0000000-05:00","details":"smtp;250 2.6.0 <0578c155-7dc1-41af-bad8-590eb31ed810@mtasv.net> [InternalId=30760555796733, Hostname=PH7PR11MB6722.namprd11.prod.outlook.com] 64797 bytes in 0.183, 345.611 KB/sec Queued mail for delivery -> 250 2.1.5"}]}]',	1,	'2025-11-20 21:04:09',	'2025-11-20 21:04:09'),
(24,	137795,	'FELE',	168761,	'pmsdian_getdocumentmaillog',	'luisalbertopanameno@hotmail.com',	'2025-11-20 17:41:30',	'Sent',	'["z09004479690002500046532.zip"]',	'[{"recipients":["luisalbertopanameno@hotmail.com"],"receivedAt":"2025-11-20T17:41:30-05:00","attachments":["z09004479690002500046532.zip"],"status":"Sent","messageEvents":[{"recipient":"luisalbertopanameno@hotmail.com","type":"Delivered","receivedAt":"2025-11-20T17:41:32.0000000-05:00","details":"smtp;250 2.6.0 <f304fdca-c8ce-4589-9f42-66c1cb272552@mtasv.net> [InternalId=15676630655686, Hostname=CH8PR11MB9460.namprd11.prod.outlook.com] 65128 bytes in 0.258, 245.707 KB/sec Queued mail for delivery -> 250 2.1.5"}]}]',	1,	'2025-11-20 21:05:13',	'2025-11-20 21:05:13'),
(25,	68972,	'FELE',	168760,	'pmsdian_getdocumentmaillog',	'linaocampo1708@outlook.com',	'2025-11-20 17:41:25',	'Sent',	'["z09004479690002500046531.zip"]',	'[{"recipients":["linaocampo1708@outlook.com"],"receivedAt":"2025-11-20T17:41:25-05:00","attachments":["z09004479690002500046531.zip"],"status":"Sent","messageEvents":[{"recipient":"linaocampo1708@outlook.com","type":"Delivered","receivedAt":"2025-11-20T17:41:26.0000000-05:00","details":"smtp;250 2.6.0 <4b6ae8ca-2c87-475e-bac8-c732eeb2b80d@mtasv.net> [InternalId=170510201679170, Hostname=DM8PR02MB8088.namprd02.prod.outlook.com] 64607 bytes in 0.129, 485.564 KB/sec Queued mail for delivery -> 250 2.1.5"}]}]',	1,	'2025-11-20 21:06:37',	'2025-11-20 21:06:37'),
(26,	137824,	'FELE',	168764,	'pmsdian_getdocumentmaillog',	'josuez15@hotmail.com',	'2025-11-20 17:46:25',	'Sent',	'["z09004479690002500046536.zip"]',	'[{"recipients":["josuez15@hotmail.com"],"receivedAt":"2025-11-20T17:46:25-05:00","attachments":["z09004479690002500046536.zip"],"status":"Sent","messageEvents":[{"recipient":"josuez15@hotmail.com","type":"Delivered","receivedAt":"2025-11-20T17:46:26.0000000-05:00","details":"smtp;250 2.6.0 <5baf534c-5661-467b-ad72-35f196fdcadd@mtasv.net> [InternalId=64802466576909, Hostname=SA1PR07MB9182.namprd07.prod.outlook.com] 64597 bytes in 0.168, 375.478 KB/sec Queued mail for delivery -> 250 2.1.5"}]}]',	1,	'2025-11-20 21:06:41',	'2025-11-20 21:06:41'),
(28,	114473,	'FELE',	168755,	'pmsdian_getdocumentmaillog',	'Raulcas1903@gmail.com',	'2025-11-20 17:07:27',	'Sent',	'["z09004479690002500046526.zip"]',	'[{"recipients":["Raulcas1903@gmail.com"],"receivedAt":"2025-11-20T17:07:27-05:00","attachments":["z09004479690002500046526.zip"],"status":"Sent","messageEvents":[{"recipient":"Raulcas1903@gmail.com","type":"Delivered","receivedAt":"2025-11-20T17:07:30.0000000-05:00","details":"smtp;250 2.0.0 OK 1763676450 6a1803df08f44-8846e5f2f2esi13524586d6.646 - gsmtp"},{"recipient":"Raulcas1903@gmail.com","type":"Opened","receivedAt":"2025-11-20T22:07:47.0000000Z","details":"Email opened with Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko Firefox/11.0 (via ggpht.com GoogleImageProxy)"}]}]',	2,	'2025-11-20 21:07:18',	'2025-11-20 21:07:18'),
(29,	136992,	'FELE',	168757,	'pmsdian_getdocumentmaillog',	'km120sur2003@yahoo.com',	'2025-11-20 17:23:29',	'Sent',	'["z09004479690002500046528.zip"]',	'[{"recipients":["km120sur2003@yahoo.com"],"receivedAt":"2025-11-20T17:23:29-05:00","attachments":["z09004479690002500046528.zip"],"status":"Sent","messageEvents":[{"recipient":"km120sur2003@yahoo.com","type":"Delivered","receivedAt":"2025-11-20T17:23:29.0000000-05:00","details":"smtp;250 ok dirdel"},{"recipient":"km120sur2003@yahoo.com","type":"Opened","receivedAt":"2025-11-20T23:31:40.0000000Z","details":"Email opened with Mozilla/5.0 (Linux; Android 10; SM-G960U1 Build/QP1A.190711.020; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/142.0.7444.102 Mobile Safari/537.36 from Pradera, Departamento del Huila"}]}]',	2,	'2025-11-20 21:07:40',	'2025-11-20 21:07:40'),
(30,	114473,	'FELE',	168752,	'pmsdian_getdocumentmaillog',	'Raulcas1903@gmail.com',	'2025-11-20 16:56:30',	'Sent',	'["z09004479690002500046523.zip"]',	'[{"recipients":["Raulcas1903@gmail.com"],"receivedAt":"2025-11-20T16:56:30-05:00","attachments":["z09004479690002500046523.zip"],"status":"Sent","messageEvents":[{"recipient":"Raulcas1903@gmail.com","type":"Delivered","receivedAt":"2025-11-20T16:56:33.0000000-05:00","details":"smtp;250 2.0.0 OK 1763675793 af79cd13be357-8b329598d40si97143885a.685 - gsmtp"}]}]',	1,	'2025-11-20 21:22:24',	'2025-11-20 21:22:24'),
(31,	137293,	'FELE',	168731,	'pmsdian_getdocumentmaillog',	'eduardomedinavalencia2@gmail.com',	'2025-11-20 15:27:27',	'Sent',	'["z09004479690002500046502.zip"]',	'[{"recipients":["eduardomedinavalencia2@gmail.com"],"receivedAt":"2025-11-20T15:27:27-05:00","attachments":["z09004479690002500046502.zip"],"status":"Sent","messageEvents":[{"recipient":"eduardomedinavalencia2@gmail.com","type":"Delivered","receivedAt":"2025-11-20T15:27:31.0000000-05:00","details":"smtp;250 2.0.0 OK 1763670451 6a1803df08f44-8846e652d2csi11499816d6.1153 - gsmtp"}]}]',	1,	'2025-11-20 21:22:32',	'2025-11-20 21:22:32'),
(32,	137825,	'FELE',	168733,	'pmsdian_getdocumentmaillog',	'FACTURACION@GMAIL.COM',	'2025-11-20 15:40:29',	'Sent',	'["z09004479690002500046504.zip"]',	'[{"recipients":["FACTURACION@GMAIL.COM"],"receivedAt":"2025-11-20T15:40:29-05:00","attachments":["z09004479690002500046504.zip"],"status":"Sent","messageEvents":[{"recipient":"FACTURACION@GMAIL.COM","type":"Delivered","receivedAt":"2025-11-20T15:40:31.0000000-05:00","details":"smtp;250 2.0.0 OK 1763671231 af79cd13be357-8b3295bac61si93603885a.1025 - gsmtp"}]}]',	1,	'2025-11-20 21:22:37',	'2025-11-20 21:22:37'),
(33,	44181,	'FELE',	168769,	'pmsdian_getdocumentmaillog',	'KELLYANGELOS1988@GMAIL.COM',	'2025-11-20 18:15:25',	'Sent',	'["z09004479690002500046541.zip"]',	'[{"recipients":["KELLYANGELOS1988@GMAIL.COM"],"receivedAt":"2025-11-20T18:15:25-05:00","attachments":["z09004479690002500046541.zip"],"status":"Sent","messageEvents":[{"recipient":"KELLYANGELOS1988@GMAIL.COM","type":"Delivered","receivedAt":"2025-11-20T18:15:27.0000000-05:00","details":"smtp;250 2.0.0 OK 1763680527 d75a77b69052e-4ee48ef668asi13592911cf.1435 - gsmtp"}]}]',	1,	'2025-11-20 21:28:16',	'2025-11-20 21:28:16'),
(34,	137821,	'FELE',	168768,	'pmsdian_getdocumentmaillog',	'jhonbriangomezvictoria81@gmail.com',	'2025-11-20 18:08:25',	'Sent',	'["z09004479690002500046540.zip"]',	'[{"recipients":["jhonbriangomezvictoria81@gmail.com"],"receivedAt":"2025-11-20T18:08:25-05:00","attachments":["z09004479690002500046540.zip"],"status":"Sent","messageEvents":[{"recipient":"jhonbriangomezvictoria81@gmail.com","type":"Delivered","receivedAt":"2025-11-20T18:08:26.0000000","details":"smtp;250 2.0.0 OK 1763680106 e9e14a558f8ab-435a9056559si28420435ab.1 - gsmtp"},{"recipient":"jhonbriangomezvictoria81@gmail.com","type":"Opened","receivedAt":"2025-11-20T18:16:14.0000000","details":"Email opened with Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko Firefox/11.0 (via ggpht.com GoogleImageProxy)"}]}]',	2,	'2025-11-20 21:28:20',	'2025-11-20 21:28:20'),
(35,	44181,	'FELE',	168767,	'pmsdian_getdocumentmaillog',	'KELLYANGELOS1988@GMAIL.COM',	'2025-11-20 18:04:25',	'Sent',	'["z09004479690002500046539.zip"]',	'[{"recipients":["KELLYANGELOS1988@GMAIL.COM"],"receivedAt":"2025-11-20T18:04:25-05:00","attachments":["z09004479690002500046539.zip"],"status":"Sent","messageEvents":[{"recipient":"KELLYANGELOS1988@GMAIL.COM","type":"Delivered","receivedAt":"2025-11-20T18:04:27.0000000-05:00","details":"smtp;250 2.0.0 OK 1763679867 6a1803df08f44-8846e65d41dsi13765786d6.1267 - gsmtp"},{"recipient":"KELLYANGELOS1988@GMAIL.COM","type":"Opened","receivedAt":"2025-11-20T23:05:04.0000000Z","details":"Email opened with Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko Firefox/11.0 (via ggpht.com GoogleImageProxy)"}]}]',	2,	'2025-11-20 21:28:25',	'2025-11-20 21:28:25'),
(38,	124138,	'FELE',	168803,	'pmsdian_getdocumentmaillog',	'claudiazorrilla1980@gmail.com',	'2025-11-21 09:59:26',	'Sent',	'["z09004479690002500046575.zip"]',	'[{"recipients":["claudiazorrilla1980@gmail.com"],"receivedAt":"2025-11-21T09:59:26-05:00","attachments":["z09004479690002500046575.zip"],"status":"Sent","messageEvents":[{"recipient":"claudiazorrilla1980@gmail.com","type":"Delivered","receivedAt":"2025-11-21T09:59:27.0000000-05:00","details":"smtp;250 2.0.0 OK 1763737167 6a1803df08f44-8846e49b804si20248606d6.81 - gsmtp"}]}]',	1,	'2025-11-21 10:02:08',	'2025-11-21 10:02:08'),
(39,	55697,	'FELE',	168804,	'pmsdian_getdocumentmaillog',	'maribelparedes8167@gmail.com',	'2025-11-21 10:04:27',	'Sent',	'["z09004479690002500046576.zip"]',	'[{"recipients":["maribelparedes8167@gmail.com"],"receivedAt":"2025-11-21T10:04:27-05:00","attachments":["z09004479690002500046576.zip"],"status":"Sent","messageEvents":[{"recipient":"maribelparedes8167@gmail.com","type":"Delivered","receivedAt":"2025-11-21T10:04:29.0000000-05:00","details":"smtp;250 2.0.0 OK 1763737469 46e09a7af769-7c78d4210e1si1119297a34.452 - gsmtp"}]}]',	1,	'2025-11-21 10:07:52',	'2025-11-21 10:07:52');
















INSERT INTO "gbl_email_log_pmsdian_event" ("id", "gbl_email_log_pmsdian_id", "sha256", "recipient", "type", "received_at", "details", "created_at") VALUES
(21,	36,	'2f16aeb497d1d0e6efa5bc890f2fb52bf5e2ad58afeade376528799470a37aee',	'dianita7811@hotmail.com',	'Delivered',	'2025-11-20 17:44:40',	'smtp;250 2.6.0 <e9097a54-0b52-4960-bbd7-162aa523f9fe@mtasv.net> [InternalId=122355028344961, Hostname=SN7PR12MB7452.namprd12.prod.outlook.com] 65306 bytes in 0.623, 102.217 KB/sec Queued mail for delivery -> 250 2.1.5',	'2025-11-20 21:36:07'),
(22,	36,	'0bf697d50daf9812c8c5a13829b6f5a9375ea7b63751b457d37d82d2e0d4553e',	'carlos.calderon@pacific-hs.com',	'Delivered',	'2025-11-20 21:37:20',	'smtp;250 2.0.0 OK 1763692640 d75a77b69052e-4ee48e86435si13293011cf.777 - gsmtp',	'2025-11-20 21:37:45'),
(23,	36,	'82e37cb6b8b153cadc6729e39a6e7d3bb9158080a8af7d3ac67a3258c490879d',	'carlos.calderon@pacific-hs.com',	'Delivered',	'2025-11-20 21:41:35',	'smtp;250 2.0.0 OK 1763692895 d75a77b69052e-4ee48d19aacsi14375621cf.66 - gsmtp',	'2025-11-20 21:42:30'),
(24,	36,	'132381b3a6efd6394f13930abf4a426b15eb5c66e9d135e727e5c635dde0df29',	'carlos.calderon01@outlook.com',	'Delivered',	'2025-11-20 21:41:36',	'smtp;250 2.6.0 <ae027887-6d7d-4955-870c-5a213050259f@mtasv.net> [InternalId=10286446712210, Hostname=LV4PR05MB11200.namprd05.prod.outlook.com] 64654 bytes in 0.222, 284.250 KB/sec Queued mail for delivery -> 250 2.1.5',	'2025-11-20 21:42:30'),
(25,	36,	'c5867dc4c334f96f24814d700e4712767cbbe35d34a69a3a594165b8b328754a',	'carlos.calderon@pacific-hs.com',	'Opened',	'2025-11-21 02:41:47',	'Email opened with Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko Firefox/11.0 (via ggpht.com GoogleImageProxy)',	'2025-11-20 21:42:30'),
(26,	36,	'6e304822d5c6e7cdc09d91eb06bd7757132a0266f14a9a6efc087f62b5211a97',	'carlos.calderon01@outlook.com',	'Opened',	'2025-11-21 02:42:04',	'Email opened with Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36 from Santiago de Cali, Departamento del Valle del Cauca',	'2025-11-20 21:42:30');




136991