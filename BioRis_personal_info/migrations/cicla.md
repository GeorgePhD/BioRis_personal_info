## Cicla = clinica las amapolas

https://cicla.cui.date/login.php
password: D5rmst5t55 o b7Od8a1m

# entrar a la base de datos de cicla

psql -U postgres -W -h localhost cicla (Trintrilen2026\*)

atajos proxy.sql cuidate

## wzp message:

[15:54, 3/23/2026] George: para Cicla tenemos que Dicomizar los pdf que contienen los informes, ahí matias es quien cacha el proceso de dicomizado para integrarlos al archivo Dicom con las imágenes, creo que como modalidad Doc
[15:54, 3/23/2026] George: Mauricio said this
[16:00, 3/23/2026] Cuidate Matias: en es ecaso deberias coenctar a su base de datos,debes usar atajos proxy.sql cuidate y ahi con usuario postgres:Trintrilen2026\*, buscar la base de datos de cicla primero.

# step 2 , DB connection

- dbeaver on terminal, this runs the app.
- atajos proxy.sql cuidate, this runs the db connection.
- con usuario postgres:Trintrilen2026\*(password), buscar la base de datos de cicla primero.

calendar_exams
report_history_state (status: 1 y 2?)
history_status

## Matias's comments: tienes que asociar tabla calendar_exam con report_history y ver los informes de cada paciente:

tabla calendar
patient(id,dni) - no dni!?
calendar(patient,id)
calendar_exam(calendar,id,history parece)
report_history(calendar)
con eso debes porbar hacer select

## Consulta sql

select patient.id as patient_id, calendar_exam.id as calendar_exam_id, calendar_exam.history as calendar_exam_history, calendar_exam.calendar as calendar_exam_calendar, report_history.calendar as report_history_calendar
from patient
join calendar
on patient.id = calendar.patient
join calendar_exam
on calendar.id = calendar_exam.calendar
join report_history
on calendar.id = report_history.calendar;

## matias: usa history de calendar_exam para unirlo con report_history.id

## deberias crear un script que genere un json con esos datos pro paciente, asi despues tambien deberias ver como obntener su informe pdf

## guarda en tu pc, genera un archivo que gener ese archivo json,asi despues cuando tengas que descragar los pdf, tienes una mini base de datos

## deberias alamcenarlo y tener alguan referecnia, de que pacienets le pertenecesn cuales pdf, ais despues sigue el apso de asociar el calendar con un study_uid

# build json file with patients' information

- it will show a list, right click on one and select "export data" and follow instructions

SELECT json_build_object(
'paciente_id', patient.id,
'historial_clinico', json_agg(
json_build_object(
'examen_id', calendar_exam.id,
'nombre_examen', calendar_exam.exam,
'reporte_id', report_history.id,
'fecha_reporte', report_history.date
)
)
)
FROM patient
JOIN calendar
ON patient.id = calendar.patient
JOIN calendar_exam
ON calendar.id = calendar_exam.calendar
JOIN report_history
ON calendar_exam.history = report_history.id
GROUP BY patient.id;

always first:

- docker ps - only if not running: sudo systemctl start docker or sudo systemctl enable docker

## 1er paso, descargar la lista de estudios. csv,uiu, patientid.

- atajos dicom = starts the process (toolkit) of dicomizing the pdfs - http://localhost:9595 = web interface (toolkit) to see the dicomized pdfs
  inside toolkit: 1. modulo findscu -> request -> host: latam4.storage.bio, port: 9200, aetitle: CUI_DATE, call: CICLA. 2. enter date range, patient id, modality, etc. and click search. 3. download the dicomized pdfs. -> export csv
- atajos proxy.sql cuidate = connect to DB (Abrir proxy hacia google cloud sql)
- connect through dbeaver

## 2 buscar accession_no, report history, en sql se busca.

    - calendar -> calendar.id = accession_no
    - calendar_exam.calendar = calendar.id

# succession_no (calendar.id) from cicla, solo despachados y válidados.

SELECT id as calendar_id_for_succession_no
from calendar
where exam_state in ('validado', 'despachado');

## Cicla modo remoto

## abrir con chrome cerrado

1. atajos cuidate2.red = Abrir puerto para navegar en red cuidate chile, puerto 9001
2. atajos chrome.proxy -> opens chrome with proxy (tunnel)
3. http://192.168.0.10:9595/v2/ui#/echo
4. advanced tools: monitor de tareas
5. comparar: source node cicla-cuidate / target node cicla-codesud

## envíos

6. date YYYYMMDD 19600101-20211231 (LAST SENT = 19600101-20221231) -> differences: 11464
   17042026: 20230101-20231231 -> differences 7351 -> Job ID fbdabe79
   17042026: 20240101-20241231 -> differences 7338 -> Job ID ff691474
   20042026: 20250101-20251231 -> differences 8XXX -> missed
   22042026: 20260101-20260331 -> differences 1241 -> Job ID 7850af41

7. select studies to be sent -> move all
8.

### "Dicomizar" un informe o un PDF significa convertir un documento estándar (que suele estar fuera del flujo de trabajo médico estructurado) en un objeto DICOM (Digital Imaging and Communications in Medicine).

3 dicomizar informes/pdf para dicomizar con api
4 migrar la informacion
encontrar los pdfs por paciente, crear script que geenra archivo csv.
enviar iamgenees al nodo que nos dejaron
crar script que se meta a la DB para ver el pdf que se genera para hacer la decomizacion
el pdf viene de otro lado, viene de una integracion
son visibles en el sistema
tenemos un pdf
generar el json, saber cual es el pdf, levantar el toolkit (programa que hizo matias, findscu, dicomizer), revisar y decomizar
enviar a latam4
conectar con el newpacs
atajos dicom = descarga el dicom/programa
localhost 9595
cicla.cui.date/index.php
falta conectar las partes
enviar desde el server, no del local. Matias instalará el toolkit en el server.

1. check docker is running = docker ps
2. start docker = s
3.
4.

## https://cicla.cui.date/index.php

1. traza -> traza completa -> despachados
1. traza -> traza completa -> d

calendar_exam.history = report_history.id

select \* from calendar_exam where calendar_exam.exam_state in ('validado', 'despachado') order by id desc limit 100;

create script to get all the studies that are in 'validado' or 'despachado' state and download the pdf.

https://cicla.cui.date/inc/modules/report/viewer.php?history_view=37161&module=

# atajos cuidate.bioris, en carpeta /server/data/repositories/salida crea script o lo que necesites

# command to download pdfs from cicla using the url

cut -d',' -f9 calendar*exam_202604161717.csv | tail -n +2 | parallel -j 5 --retries 5 --bar --joblog progreso.log 'curl -f -sSL "https://cicla.cui.date/inc/modules/report/viewer.php?history_view={}&module=" -o /server/data/repositories/salida/pdfs_cicla_migration_16042026/reporte*{}.pdf || echo "ID {}: Falló con código $?" >> /server/data/repositories/salida/errores_descarga.txt'

# download pdfs into own pc

## cut -d',' -f9 Downloads/calendar*exam_202604161717.csv | tail -n +2 | tr -d '\r' | parallel -j 5 --bar --joblog progreso.log '/usr/bin/curl -f -sSL "https://cicla.cui.date/inc/modules/report/viewer.php?history_view={}&module=" -o /home/george/pdfs_cicla/reporte*{}.pdf'

### now since the downloaded pdf files came with html body, we need to rename them to html.

find . -maxdepth 1 -name "\*.pdf" | parallel --bar 'mv {} {.}.html'

### now to convert them to real pdf files

## check db en Dbeaver = atajos proxy.sql cuidate (check that proxy is installed : atajos proxy.sql.download.linux)

columnas: 41.869

## entrar en PCS -> toth -> todos -> cuidate-latam-1

upload files from console to gcloud
gcloud compute scp --project=prod-cuidate-redsalud Downloads/SITE_PPS_202603031342.csv cuidate-chile:/home/juan/ --tunnel-through-iap --compress

pdf necesita ser guardado con el study_uid, pero como sabemos cual es el study_uid quye le cprrespomnde a cada pdf.
el estudio dicom que tiene un study_uid tiene un numero/columna , tabla calendar se llama ID , en la tabla calendar_exam se llama calendar y en la tabla report_history se llama calendar.

en el ris siempre se llama calendar

# STEPS TO GET UIDs

1. get accession_no from calendar_exam = accession_no === calendar.id
2. get accession numbers for patients and (atenciones)
3. example study_uid = 1.2.345.patient_number.history_number

# use api to get study_uid = https://cicla.cui.date/api/v1/studies (correct url?)

### check duplicate history numbers

1.2.0.123.445.456 = study_uid
