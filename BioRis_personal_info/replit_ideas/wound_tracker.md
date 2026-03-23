# Wound-Tracker

## layout
Construye una aplicación web llamada “Patient Wound Progression Tracker”.

La aplicación debe ser un portal simple para equipos de salud donde puedan subir fotografías de heridas o lesiones de un paciente a lo largo del tiempo y visualizar la progresión.

No inventes información médica ni diagnósticos. La aplicación solo debe analizar cambios visuales objetivos en las imágenes (tamaño, color y textura). Si se incluyen explicaciones médicas, deben basarse únicamente en conocimiento médico general y reconocido.

## logic

- Objetivo de la aplicación

Permitir que profesionales de salud:

Registren pacientes.

Suban fotos de heridas o lesiones con fecha.

Comparen automáticamente imágenes anteriores y actuales.

Visualicen cambios detectados.

Generen una línea de tiempo visual del progreso de la herida.

Esta herramienta está pensada para:

cuidado de heridas crónicas

seguimiento post-operatorio

dermatología

monitoreo remoto de pacientes

Funcionalidades principales
1. Gestión de pacientes

Crear un sistema básico de pacientes con:

Campos:

patient_id

nombre

fecha de nacimiento

notas clínicas opcionales

Un paciente puede tener múltiples imágenes asociadas.

2. Subida de imágenes

Permitir subir imágenes con:

fecha automática

etiqueta opcional

comentario clínico opcional

Cada imagen debe almacenarse con:

patient_id

image_path

upload_date

note

3. Comparación automática de imágenes

Cuando se sube una nueva imagen:

Compararla con la imagen anterior usando procesamiento de imágenes con OpenCV.

Detectar cambios en:

Tamaño

comparar área aproximada de la lesión

Color

análisis de histograma de color

cambios en tonalidades rojizas, oscuras o amarillas

Textura

análisis básico de bordes y contraste

Generar:

porcentaje estimado de cambio

mapa visual resaltando diferencias

No generar diagnósticos médicos.

4. Timeline visual

Crear una línea de tiempo del paciente que muestre:

fecha

miniatura de la imagen

cambios detectados respecto a la anterior

Formato tipo:

[01 Feb]  Foto inicial
[05 Feb]  +12% área | cambio color
[10 Feb]  -8% área | textura más uniforme
5. Reporte visual

Generar un reporte simple que incluya:

imágenes ordenadas cronológicamente

gráficos de evolución del tamaño de la lesión

resumen automático de cambios detectados

El reporte puede exportarse como:

PDF

o página web imprimible.

Requisitos técnicos

Usar stack simple:

Frontend:

React

Tailwind

Backend:

Python Flask o FastAPI

Procesamiento de imágenes:

OpenCV

NumPy

Base de datos:

SQLite o PostgreSQL

Almacenamiento de imágenes:

carpeta local /uploads

Reglas importantes

No inventar información clínica.

No diagnosticar enfermedades.

El sistema solo detecta cambios visuales.

Indicar que los resultados no reemplazan evaluación médica profesional.

UI mínima

Pantallas:

Lista de pacientes

Perfil del paciente

Subir nueva imagen

Timeline de evolución

Vista de comparación entre dos imágenes

Función avanzada opcional

Si es posible implementar:

overlay de imágenes para comparar

slider interactivo “before / after”

heatmap de diferencias

Resultado esperado

Una aplicación funcional donde se pueda:

Crear paciente

Subir fotos de heridas

Detectar cambios visuales automáticamente

Ver la progresión en una línea de tiempo

Generar reportes visuales

Si quieres, también puedo darte una versión mucho más potente del prompt (nivel startup / hospital real) que incluye:

segmentación automática de heridas con IA

medición en milímetros usando calibración

integración con historias clínicas (FHIR / HL7)

análisis clínico asistido por IA.