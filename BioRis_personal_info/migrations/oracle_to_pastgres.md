TODO: carptea oracle, inside esta el script. gemini respladar la info. redsalud. ask Juan francisco about steps to do.

Oka. Por mientras hacer la migración de la BD de Oracle a una de postgres

Confirmado: el volumen montado en /mnt/ tiene 441 GB disponibles, lo cual es más que suficiente para la descarga
  (actualmente solo ocupa 25 MB).

  El proceso continúa ejecutándose en segundo plano y guardando todo en /mnt/estudios/database_dump/. He verificado
  que los permisos son correctos y el script está operando con normalidad.
 > cuanto demorará y como veo el proceso en cuanto va?

 ✦ Para ver el progreso y estimar el tiempo, aquí tienes los detalles actuales:

  1. ¿Cómo ver por dónde va?
  Puedes monitorear el progreso en tiempo real usando estos comandos:

   * Ver la última tabla completada:
   1     tail -f export.log
   * Contar cuántas tablas de las 626 ya se descargaron:

   1     ls /mnt/estudios/database_dump/*.csv | wc -l
   * Ver el tamaño total acumulado:
   1     du -sh /mnt/estudios/database_dump/

  2. Estimación de tiempo
  El proceso ha completado las primeras tablas pequeñas rápidamente, pero ahora está procesando las tablas más
  pesadas.

   * Estado actual: Ha procesado unas 10-15 tablas de 626.
   * Tablas Críticas: La tabla ADDRESS (25MB y 245k filas en la prueba inicial) es una de las más grandes. Si esa
     tabla tarda ~10 minutos en total, y hay varias similares, el proceso completo podría tomar entre 4 y 8 horas.
   * Factor variable: El tiempo dependerá de cuántas tablas tengan millones de filas y de la velocidad de respuesta
     del servidor Oracle. 

     Cómo verificarlo cuando vuelvas a encender tu PC:
  Cuando vuelvas a conectarte, puedes comprobar que el proceso sigue vivo con este comando:
   1 ps aux | grep export_database.py | grep -v grep
  Si el comando te devuelve una línea con un número de proceso (PID), significa que sigue trabajando.

  Resumen de seguridad:
   * Si apagas tu PC: La descarga sigue.
   * Si se corta el internet: La descarga sigue.
   * Si el servidor de base de datos se reinicia: El script se detendrá, pero gracias al sistema de "resume" que
     programé, cuando lo volvamos a lanzar continuará exactamente donde se quedó, sin duplicar archivos.

  Puedes irte tranquilo, el sistema está trabajando de forma autónoma.

  
  # connect to oracle db
  
  gcloud compute ssh --project=prod-cuidate-redsalud cuidate-arc-1 --ssh-flag="-vvv" --ssh-flag="-L 1521:10.1.210.102:1521 -g" -- -N

  