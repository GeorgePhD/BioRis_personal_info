# HRT
hospital regional de talca

# Google Cloud
infrastructure informatica de la empresa.
 
# Cloud SQL
base de datos de la empresa.


# Security Command Center
centro de comando de seguridad de la empresa.

# Cloud Armor
proteccion de la infraestructura de la empresa.

# DICOM (Digital Imaging and Communications in Medicine)
empresa de imagenes medicas.

# DCMDK

## Ris Radiology Information System
## PACKS Packages Management System
    - Ejemplo:

Pack básico (agenda + pacientes)

Pack avanzado (informes + integración HL7 + estadísticas) 

## MIRT Middleware (Protocolos médicos)

## RCE = Registro Clínico Electrónico




# Cuidate Commands Done Working
# screen name = jorgeEnvioTotalRestante
## change screen name = screen -S IdNumber -X sessionname newName
- cat listas/provi_rm.csv | awk -F ',' '{print $1}' | parallel --jobs 10 --bar --progress ./exist.sh {}
def: EEl comando cat listas/provi_rm.csv | awk -F ',' '{print $1}' | parallel --jobs 10 --bar --progress ./exist.sh {} construye un pipeline de procesamiento donde primero se lee un archivo CSV, luego se extrae su primera columna y finalmente se ejecuta un script en paralelo para cada valor obtenido. cat listas/provi_rm.csv abre y envía el contenido completo del archivo provi_rm.csv al stdout; el símbolo | (pipe) redirige esa salida al siguiente comando sin crear archivos intermedios; awk -F ',' '{print $1}' es un procesador de texto donde -F ',' define la coma como separador de campos (lo que corresponde a la estructura típica de un CSV) y {print $1} indica que se debe imprimir solo el primer campo de cada línea, produciendo una lista de valores de la primera columna; esa lista pasa por otro | hacia GNU Parallel, que es una herramienta para ejecutar comandos concurrentemente; la opción --jobs 10 indica que se ejecutarán hasta 10 procesos simultáneos, --bar muestra una barra visual de progreso, --progress imprime estadísticas adicionales sobre los trabajos ejecutados (cuántos han terminado, cuántos están en ejecución, tiempo estimado, etc.), y ./exist.sh {} es el comando que se ejecutará para cada línea recibida, donde {} es un placeholder que GNU Parallel reemplaza con cada valor producido por awk, de modo que el script exist.sh se ejecuta una vez por cada elemento de la primera columna del CSV, procesando múltiples entradas al mismo tiempo gracias al paralelismo.

- cat listas/provi_rm.csv | awk -F ',' '{print $1}' | head -n 100 | parallel --jobs 10 --bar --progress ./exist.sh {}
def: El comando cat listas/provi_rm.csv | awk -F ',' '{print $1}' | head -n 100 | parallel --jobs 10 --bar --progress ./exist.sh {} construye un pipeline de procesamiento de datos donde se leen registros desde un archivo CSV, se extrae su primera columna, se limita la cantidad de entradas y luego se procesan en paralelo. cat listas/provi_rm.csv abre el archivo y envía todo su contenido al stdout; el operador | (pipe) redirige esa salida al siguiente comando; awk -F ',' '{print $1}' es un procesador de texto donde -F ',' define la coma como separador de campos (adecuado para archivos CSV) y {print $1} indica que se imprima únicamente el primer campo de cada línea, generando una lista con los valores de la primera columna; el siguiente comando head -n 100 toma solo las primeras 100 líneas de esa lista, lo que normalmente se usa para pruebas o para procesar un subconjunto del dataset; luego otro | envía esas 100 entradas a GNU Parallel, donde parallel --jobs 10 indica que se ejecutarán hasta 10 procesos simultáneos, --bar muestra una barra visual de progreso, --progress imprime estadísticas adicionales del avance de los trabajos, y finalmente ./exist.sh {} es el comando que se ejecutará para cada entrada, donde {} es un placeholder que GNU Parallel reemplaza por cada valor recibido (proveniente de la primera columna del CSV), de modo que el script exist.sh se ejecuta una vez por cada uno de los primeros 100 valores, procesándolos concurrentemente con un máximo de 10 ejecuciones en paralelo.



- screen -ls = consoles working on background
- screen -r 3784220 = enter console

- history = shows the history of commands

- cat listas/provi_rm.csv | awk -F ',' '{print $1}' | head -n 10

- entrar consola toth:
    - 1st time console opened from user george:
        - do:
            - atajos redsalud.chile = you enter george@cuidate-chile:~$
            - tajos find redsalud = shows menu 
            - sudo su toth = change to toth user, here there are projects and scripts
    - when in toth: cd to go back and be able to see the folders/files
    - see orthanc and inside you must see 


- cat listas/provi_rm.csv | awk -F ',' '{print $1}'

# Checks the 100 first rows and saves the output in a file called provitest100primeros
- tail -n +2 listas/provi_rm.csv | head -100 | cut -d',' -f1 | parallel --jobs 30 --bar --progress ./automatic.py {} AGFA_MIG provitest100primeros

- tail +102 listas/provi_rm.csv | head -n 102 | cut -d ',' -f1 | parallel --jobs 30 --bar --progress ./exist-2.sh {} AGFA_MIG Jorge200i

- 


- this saves the output in a file called jorgeFrom1201.txt

- cp fileName /tmp
- cloud.google.com -> compute engine -> vm instances -> cuidate-chile -> SSH -> type the route to the file 

## when in screen mode
ctrl + A D = detach from console
flecha arriba shows = screen -r siguientes100

ncdu = program to see visual df -h
make up = ? (on console)

## Monitor data transfer
- nload -m -u M

primary internal 10.138.0.2
external 34.105.111.196
136.117.132.74
https://storage.googleapis.com/qwiklabs-gcp-04-2bf21d3adb64/my-excellent-blog.png




https://meddream.com/documentation/installation-integration-guide/integration-via-communication-api/

https://events.zoom.us/ev/Apn73btuO7d7ipBGPvrm076Kw9UOr_YBCrkjnfnnCxTwuJ0axmdh~Ai5pQj04a1OM1Ws6V2jWHztKb0Fa249HG-otGP1AFerOiUHQ7xqGGYQz8Kzd7sINasUD_KZMWqgpBkQuIXWBpVAFUg

TECHNICAL TRAINING: MedDream Communication API integration | Tue, Mar 31, 2026 4:00 PM EEST

TECHNICAL TRAINING: MedDream Communication API integration
Tue, Mar 31, 2026 10:00 AM - 11:30 AM -03
Organized by MedDream

puedes revisar los videos de un canal de youtube y buscar solo donde el titulo tenga que ver con "integracion" ? necesitamos encontrar el video donde haya un tutorial, si es que lo hay, para integrar el viewer de la compañia. se supone que es un html5 que deberiamos integrar en nuestro proyecto. Busca acá:  

