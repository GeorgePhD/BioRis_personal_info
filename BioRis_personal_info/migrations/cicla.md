## Cicla = clinica las amapolas

# commands DB
- \l = list databases
- \c nombre_db = connect to database
- \dt = list tables (Si no aparece nada:probablemente estás en otro esquema (muy común))
- \dt *.* = list all tables/schemes
- \dn = list schemes
- \d nombre_tabla = describe table

Ejemplo:

\dt ✔ (psql)
SELECT ... ✔ (SQL estándar)


# entrar a la base de datos de cicla
psql -U postgres -W -h localhost cicla (Trintrilen2026*)

atajos proxy.sql cuidate : showed error at first.
 - proxy was missing and it needed to be installed/download = atajos proxy.sql.download.linux|proxy.sql.download.mac.m1|proxy.sql.download.mac.x64

 atajos proxy.sql.download.linux =  this was needed only

 first try to login for auth:

 gcloud auth application-default login
Go to the following link in your browser, and complete the sign-in prompts:

    https://accounts.google.com/o/oauth2/auth?response_type=code&client_id=764086051850-6qr4p6gpi6hn506pt8ejuq83di341hur.apps.googleusercontent.com&redirect_uri=https%3A%2F%2Fsdk.cloud.google.com%2Fapplicationdefaultauthcode.html&scope=openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fsqlservice.login&state=raW5sLTrHDL5XO1AZfEns3ya2Jd48D&prompt=consent&token_usage=remote&access_type=offline&code_challenge=5e2O3L9Fy9KZVGQlh2zbuuy61jwnoTy_pJT2bgcjKOo&code_challenge_method=S256

Once finished, enter the verification code provided in your browser:

it shows this: Sign in to the gcloud CLI
You are seeing this page because you ran the following command in the gcloud CLI from this or another machine. If this is not the case, close this tab.

gcloud auth application-default login --no-launch-browser

Enter the following authorization code in gcloud CLI on the machine you want to log into. This is a credential similar to your password and should not be shared with others.

gcloud credentials:
4/0Aci98E_HfYf7aoOZs4Z3KM-hK5_qX9ggVeeF0LD5FguY8KWYXYFVCE_x1Eipp0I4-qWZCA

the previous worked!

# this solved it: atajos gcloud.install.docker

## wzp message: 
[15:54, 3/23/2026] George: para Cicla tenemos que Dicomizar los pdf que contienen los informes, ahí matias es quien cacha el proceso de dicomizado para integrarlos al  archivo Dicom con las imágenes,  creo que como modalidad Doc
[15:54, 3/23/2026] George: Mauricio said this
[16:00, 3/23/2026] Cuidate Matias: en es ecaso deberias coenctar a su base de datos,debes usar atajos proxy.sql cuidate y ahi con usuario postgres:Trintrilen2026*, buscar la base de datos de cicla primero.

# step 2 , DB connection

- dbeaver on terminal, this runs the app.
- atajos proxy.sql cuidate, this runs the db connection.
- con usuario postgres:Trintrilen2026*(password), buscar la base de datos de cicla primero.

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

## Consulta sql suggested by gemini fast

SELECT 
    p.dni, 
    p.id AS patient_id, 
    ce.id AS exam_id, 
    rh.calendar AS report_calendar,
    -- Aquí puedes agregar las columnas específicas de los informes
    rh.* FROM patient p
JOIN calendar c ON p.id = c.patient
JOIN calendar_exam ce ON c.id = ce.calendar
JOIN report_history rh ON c.id = rh.calendar;

# improved: 

select patient.id, calendar_exam.id, calendar_exam.history, calendar_exam.calendar, report_history.calendar 
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




## Cicla Meeting (Clínica las amapólas) 15/04/2026
always first: 
- docker ps
        - only if not running: sudo systemctl start docker or sudo systemctl enable docker

## 1er paso, descargar la lista de estudios. csv,uiu, patientid.
- atajos dicom = starts the process (toolkit) of dicomizing the pdfs
        - http://localhost:9595 = web interface (toolkit) to see the dicomized pdfs 
        inside toolkit:
            1. modulo findscu -> request -> host: latam4.storage.bio, port: 9200, aetitle: CUI_DATE, call: CICLA.
            2. enter date range, patient id, modality, etc. and click search.
            3. download the dicomized pdfs. -> export csv

      
- atajos proxy.sql cuidate = connect to DB (Abrir proxy hacia google cloud sql)
- connect through dbeaver

## 2 buscar accession_no, report history, en sql se busca.
    - calendar -> calendar.id = accession_no

## remoto
latam4 cicla está
1. atajos cuidate2.red = Abrir puerto para navegar en red cuidate chile, puerto 9001
2. atajos chrome.proxy -> opens chrome with proxy (tunnel)
3. http://192.168.0.10:9595/v2/ui#/echo
4. advanced tools: monitor de tareas
5. comparar: source node cicla-cuidate / target node cicla-codesud

## envíos
6. date YYYYMMDD 19600101-20211231 (LAST SENT = 19600101-20221231) -> differences: 11464
17042026: 20230101-20231231 -> differences 7351 -> Job ID fbdabe79
17042026: 20240101-20241231 -> differences 7338 -> Job ID ff691474






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

select * from calendar_exam where calendar_exam.exam_state in ('validado', 'despachado') order by id desc limit 100;

create script to get all the studies that are in 'validado' or 'despachado' state and download the pdf.

https://cicla.cui.date/inc/modules/report/viewer.php?history_view=37161&module=

## atajos cuidate.bioris, en carpeta /server/data/repositories/salida  crea script o lo que necesites

# command to download pdfs from cicla using the url

## parallel -j 20 --retries 5 --joblog progreso.log 'curl -sSL "https://cicla.cui.date/inc/modules/report/viewer.php?history_view={}&module=" -o archivos_pdf/reporte_{}.pdf' :::: ids_limpios.txt

columnas: 41.869



## entrar en PCS -> toth -> todos -> cuidate-latam-1
upload files from console to gcloud
gcloud compute scp --project=prod-cuidate-redsalud Downloads/SITE_PPS_202603031342.csv cuidate-chile:/home/juan/ --tunnel-through-iap --compress