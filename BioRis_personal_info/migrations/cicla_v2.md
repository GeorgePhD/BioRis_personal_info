# Cicla (Clínica Las Amapolas) Migration Guide

This document outlines the logical steps for migrating data from the Cicla system, including database connection, report downloading, and DICOMization.

## 1. Access and Credentials

### Web Interface

- **URL**: [https://cicla.cui.date/login.php](https://cicla.cui.date/login.php)
- **Passwords**: `D5rmst5t55` or `b7Od8a1m`

### Database Connection (Postgres)

- **Command**: `psql -U postgres -W -h localhost cicla`
- **Password**: `Trintrilen2026*`
- **Proxy**: Use `atajos proxy.sql cuidate` to establish the connection.
- **Remote Database**: `postgres:Trintrilen2026*`

---

## 2. Environment and Tool Setup

### Docker

- Check if running: `docker ps`
- Start service: `sudo systemctl start docker` or `sudo systemctl enable docker`

### Toolkit (FindSCU / Dicomizer)

- **Start**: `atajos dicom`
- **Web Interface**: [http://localhost:9595](http://localhost:9595)
- **Configuration**:
  - **Host**: `latam4.storage.bio`
  - **Port**: `9200`
  - **AETitle**: `CUI_DATE`
  - **Call**: `CICLA`

### Remote Mode / Chrome Proxy

1.  Open tunnel: `atajos cuidate2.red` (Port 9001)
2.  Open Chrome with proxy: `atajos chrome.proxy` (Close Chrome before running)
3.  Web UI: [http://192.168.0.10:9595/v2/ui#/echo](http://192.168.0.10:9595/v2/ui#/echo)
4.  Monitor: Advanced tools -> Monitor de tareas.

---

## 3. Data Identification and Querying

### Core Table Relationships

- `patient.id` -> `calendar.patient`
- `calendar.id` -> `calendar_exam.calendar` (This `calendar.id` is the **Accession Number**)
- `calendar_exam.history` -> `report_history.id`

### SQL Queries

#### Identify Valid Exams for Migration

```sql
SELECT id as calendar_id_for_succession_no
FROM calendar
WHERE exam_state IN ('validado', 'despachado');
```

#### Build Patient Information JSON

```sql
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
JOIN calendar ON patient.id = calendar.patient
JOIN calendar_exam ON calendar.id = calendar_exam.calendar
JOIN report_history ON calendar_exam.history = report_history.id
GROUP BY patient.id;
```

---

## 4. PDF Acquisition and Processing

### Download Reports

Reports are downloaded using the `report_history.id` (history_view parameter).

**Command for parallel download:**

```bash
cut -d',' -f9 calendar_exam.csv | tail -n +2 | parallel -j 5 --retries 5 --bar --joblog progreso.log \
'curl -f -sSL "https://cicla.cui.date/inc/modules/report/viewer.php?history_view={}&module=" \
-o pdfs_cicla_corregidos/{}.pdf'
```

**Second command improved**

```bash
 find html_to_pdf_cicla -maxdepth 1 -name "*.pdf" -printf "%f\n" | parallel -j 5 --retries 5 --bar --joblog progreso_27042026.log 'curl -f -sSl "https://cicla.cui.date/repositories/cicla/reports/{}" -o pdfs_cicla_corregidos_27042026/{}'
```
### Fix File Extensions

Since downloaded PDFs often arrive with HTML bodies, rename them to `.html` first if necessary, then convert to proper PDF.

```bash
find . -maxdepth 1 -name "*.pdf" | parallel --bar 'mv {} {.}.html'
```

---

## 5. DICOMization and UIDs

### DICOMization Process

1.  **Generate JSON/CSV**: Create a list of patients and their corresponding PDF files.
2.  **Toolkit**: Use the Dicomizer toolkit (`atajos dicom`) to convert PDFs into DICOM objects (Modality: DOC).
3.  **Study UID Association**: Associate each PDF with its `study_uid`.
    - `accession_no` === `calendar.id`
    - Example logic: `1.2.345.patient_number.history_number`

### API Usage

- Potential endpoint for Study UIDs: `https://cicla.cui.date/api/v1/studies`

si ya tienes accession_no en tus pacientes y atenciones, debes pedir unm token a un api para token primero

curl --location 'https://latam4.storage.bio/auth/login' \
--header 'Content-Type: application/json' \
--data '{
"username":"SISTEMAS",
"client":"mauricio",
"password": "D5rmst5t55"
}'

usar accession_no,cliente,aetitleCLiente

curl --location 'https://latam4.storage.bio/studies/cicla/CICLA/accession_no/40827?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJpc3MiOiJ0b2tlbml6ZXIiLCJzdWIiOiJTSVNURU1BUyIsImlhdCI6MTc3NjcxMTc1MiwiZXhwIjoxNzc2NzI2MTUyLCJzeXN0ZW0iOiJub25lIiwiY2xpZW50IjoibWF1cmljaW8iLCJwcm9maWxlIjoibWVtb3J5In0.jKphZN6kGq-5ibiisUXAHxYmBbL9AGoIo4lrjIv1a_XOBTMEn7GlrnII6moZaidQbY3syS0mJef5GbKy-LoSsHM74XsqbWNah3S2TF7lYwdH9ZKyLYT41VMcdmsTMyugTTQSGPlMIzT8EeofHtfoWwIjYgMYzbjCWvL3LmyrvGUpsrQSXTrtgsmzAR0o5Ez0XzHUAEA0VOIzPkEP_Dzfj36y9G3P7ayEf78GBxVL-ZA-dJRVmRjkyaKJJaOEQRk7cEgfel7K91Ka2_aBa8qaEP76OXESumg_T9Vd6XQGiVj-fToAHpdpM6AJr0bLGNWx-Pu2IRb624uaicKmb_uioIqUG4P48UA2ky9ZQDSU1P6JJAWZdTGqMy4f4jNW78fK_dRtjyF7mmBMTsqki6pF5mVhF6jFD3crwJP1hMVotWgvUwi4oLoqy0eorOGdTGEYRfb78h0bPZ8kDUEonuJQ9xmZDIZx2b7-6nEvLErLdrrpwuzdlg66tgE2iEEs-g4gSE9iRWOFHuZ4CbuZ5jtLj98YGdrorx_fidhB2DHvvoybhCllMKquA2IFdzhWqLDzkdvdmrtAcYVb01G3z_aeBvnEoMwcJRnZYxRUmKyxo1fzZwojtctH0Eu-lHQyTLzXKfVo77T5fi8bssLKVg0K2jY85_LRv6FuPQiMIR9ix2Y'

---

## 6. Migration and Upload

### Upload to GCloud

```bash
gcloud compute scp --project=prod-cuidate-redsalud [FILE] cuidate-chile:/home/juan/ --tunnel-through-iap --compress
```

### Monitoring "Envíos"

Check the monitor tool to compare source (`cicla-cuidate`) and target (`cicla-codesud`) nodes.
**Recent Job History:**

- `20230101-20231231`: Job ID `fbdabe79`
- `20240101-20241231`: Job ID `ff691474`
- `20260101-20260331`: Job ID `7850af41`

---

## 7. Context and Notes (WhatsApp)

- **Mauricio**: PDF reports must be "dicomized" to integrate them into DICOM files with images (Modality: DOC). Matias knows the process.
- **Matias**:
  - Associate `calendar_exam` with `report_history` to find reports per patient.
  - Use `calendar_exam.history` to join with `report_history.id`.
  - The toolkit will be installed on the server for final execution
