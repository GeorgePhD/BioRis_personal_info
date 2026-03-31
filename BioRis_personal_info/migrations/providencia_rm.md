
# screen name = jorgeEnvioTotalRestante
## change screen name = screen -S IdNumber -X sessionname newName
- cat listas/provi_rm.csv | awk -F ',' '{print $1}' | parallel --jobs 10 --bar --progress ./exist.sh {}
def: EEl comando cat listas/provi_rm.csv | awk -F ',' '{print $1}' | parallel --jobs 10 --bar --progress ./exist.sh {} construye un pipeline de procesamiento donde primero se lee un archivo CSV, luego se extrae su primera columna y finalmente se ejecuta un script en paralelo para cada valor obtenido. cat listas/provi_rm.csv abre y envía el contenido completo del archivo provi_rm.csv al stdout; el símbolo | (pipe) redirige esa salida al siguiente comando sin crear archivos intermedios; awk -F ',' '{print $1}' es un procesador de texto donde -F ',' define la coma como separador de campos (lo que corresponde a la estructura típica de un CSV) y {print $1} indica que se debe imprimir solo el primer campo de cada línea, produciendo una lista de valores de la primera columna; esa lista pasa por otro | hacia GNU Parallel, que es una herramienta para ejecutar comandos concurrentemente; la opción --jobs 10 indica que se ejecutarán hasta 10 procesos simultáneos, --bar muestra una barra visual de progreso, --progress imprime estadísticas adicionales sobre los trabajos ejecutados (cuántos han terminado, cuántos están en ejecución, tiempo estimado, etc.), y ./exist.sh {} es el comando que se ejecutará para cada línea recibida, donde {} es un placeholder que GNU Parallel reemplaza con cada valor producido por awk, de modo que el script exist.sh se ejecuta una vez por cada elemento de la primera columna del CSV, procesando múltiples entradas al mismo tiempo gracias al paralelismo.

- cat listas/provi_rm.csv | awk -F ',' '{print $1}' | head -n 100 | parallel --jobs 10 --bar --progress ./exist.sh {}
def: El comando cat listas/provi_rm.csv | awk -F ',' '{print $1}' | head -n 100 | parallel --jobs 10 --bar --progress ./exist.sh {} construye un pipeline de procesamiento de datos donde se leen registros desde un archivo CSV, se extrae su primera columna, se limita la cantidad de entradas y luego se procesan en paralelo. cat listas/provi_rm.csv abre el archivo y envía todo su contenido al stdout; el operador | (pipe) redirige esa salida al siguiente comando; awk -F ',' '{print $1}' es un procesador de texto donde -F ',' define la coma como separador de campos (adecuado para archivos CSV) y {print $1} indica que se imprima únicamente el primer campo de cada línea, generando una lista con los valores de la primera columna; el siguiente comando head -n 100 toma solo las primeras 100 líneas de esa lista, lo que normalmente se usa para pruebas o para procesar un subconjunto del dataset; luego otro | envía esas 100 entradas a GNU Parallel, donde parallel --jobs 10 indica que se ejecutarán hasta 10 procesos simultáneos, --bar muestra una barra visual de progreso, --progress imprime estadísticas adicionales del avance de los trabajos, y finalmente ./exist.sh {} es el comando que se ejecutará para cada entrada, donde {} es un placeholder que GNU Parallel reemplaza por cada valor recibido (proveniente de la primera columna del CSV), de modo que el script exist.sh se ejecuta una vez por cada uno de los primeros 100 valores, procesándolos concurrentemente con un máximo de 10 ejecuciones en paralelo3.



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


TODO: This is next for providencia_rm migration
[10:59, 3/25/2026] Juan Francisco Del Solar: Hay que sumar los demás archivos de antes del 1201
[11:07, 3/25/2026] Juan Francisco Del Solar: Qué queda por hacer:
1. Sacar el dato exacto de lo que “no se envió”
2. ⁠Hacer la vertificación de todos los estudios para ver que es lo que falta, para la segunda iteración TODO: ./comparar_provi_csv_agfa.py --csv listas/provi_rm.csv --node AGFA_DICOM --bar --progress
3. ⁠Enviar la segunda iteración.


# uids mal formated:
que son estos: Progreso: 168020/168043 (99.99%) | Enc: 44580 | NoEnc: 123440 | Err:
   0/home/toth/orthanc/.venv/lib/python3.11/site-packages/pydicom/valuerep.py:440: UserWarning: Invalid value for
   VR UI: '27.013.372.038.692'. Please see
   <https://dicom.nema.org/medical/dicom/current/output/html/part05.html#table_6.2-1> for allowed values for each
   VR.
     warn_and_log(msg)
✦ Esas son advertencias técnicas de la librería pydicom y te explico exactamente por qué aparecen:

  1. ¿Qué significa el error?
  El estándar DICOM es muy estricto con los UIDs (los identificadores de los estudios). Dice que los números entre
  los puntos no pueden tener ceros a la izquierda (a menos que el número sea solo "0").

  En tu caso, el UID '27.013.372.038.692' tiene:
   * .013 (debería ser .13)
   * .038 (debería ser .38)

  pydicom detecta que esto es un "UID mal formado" según las reglas oficiales y te avisa.

2. ¿Cómo afecta esto a tu comparación?
   * A nivel de script: El script sigue funcionando, no se detiene.
   * A nivel de búsqueda: Es muy probable que el PACS (AGFA) no encuentre esos estudios específicos porque, al ser
     UIDs "inválidos" para el estándar, es posible que nunca se hayan guardado con ese formato exacto o que el PACS
     los haya "limpiado" al recibirlos.

  3. Sobre el progreso
  Veo que vas en 99.99% (168,020 de 168,043). ¡Ya casi termina!

  Resultados hasta ahora:
   * Encontrados: 44,580 estudios.
   * No Encontrados: 123,440 estudios.

comparar agfa_dicom cuidate: ./findscu.sh movescu_study 1.2.840.113564.99.1..54234.2018126193622527.54234.2 AGFA_DICOM CUIDATE




 
- revisé si existian los estudios en AGFA_DICOM y no estaban. 
- use el exist_study_in_agfa.sh con head y tail.
- los archivos (7)están en existe_para_comparacion.txt.notfound
esto me entregó el script: Progreso: 168043/168043 (100.00%) | Enc: 44599 | NoEnc: 123444 | Err: 0
- 


enviando el resto de estudios de providencia rm: 123.444
cat agfa_dicom_comparacion_no_encontrados.txt | parallel -j30 --progress --bar ./automatic.py {} AGFA_MIG envio_from_123444


## Nuevo automatic_v20.py: 
- cat agfa_dicom_comparacion_no_encontrados.txt | parallel -j30 --progress --bar ./automatic_v20.py {} AGFA_MIG envio_from_123444_v20


