# entrar a la base de datos de cicla
psql -U postgres -W -h localhost cicla

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
[16:00, 3/23/2026] Cuidate Matias: en es ecaso deberias coenctar a su base de datos,debes usar atajos proxy.sql cuidate y ahi con usuario postgres:Bicarbonato2017, buscar la base de datos de cicla primero.

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





