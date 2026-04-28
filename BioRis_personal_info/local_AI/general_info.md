# every AI will have its own md file with all the needed information to install them.

- check here: https://www.canirun.ai/ to see models you can install in your hardware.

Después de la experiencia que hemos tenido configurando tu RTX 3070 Ti y lidiando con la carga de los modelos, aquí tienes las "verdades universales" que debes tener en mente antes de seguir llenando tu CachyOS con IAs locales.

Estar en abril de 2026 significa que tenemos modelos muy optimizados, pero la física del hardware sigue siendo la misma.

1. La VRAM es el "Terreno Sagrado"
Como ya viste, no importa cuánta RAM tengas (tus 48GB son geniales), si el modelo no cabe en los 8GB de tu gráfica, el rendimiento cae en picado.

La Regla de Oro: Siempre busca modelos cuyo peso en GB sea al menos 1.5 GB menor que tu VRAM total. Ese espacio libre es para que tu monitor funcione y el sistema operativo no se congele.

Cuantización: En 2026, casi nadie usa modelos "Full Precision". Busca siempre versiones Q4_K_M o IQ4_XS. Son el punto dulce donde no pierdes inteligencia pero ahorras un 50% de memoria.

2. El Factor Térmico y la Factura de Luz
Correr una IA local es como jugar a un videojuego triple A en ultra, pero sin parar.

Desgaste: Tu i7-9700 y la 3070 Ti van a generar mucho calor. Si vas a dejar un agente trabajando por horas, asegúrate de tener una buena curva de ventilación en CachyOS.

Consumo: Una sesión intensa de inferencia puede consumir entre 200W y 400W constantes. Si dejas agentes corriendo todo el día, lo verás en el recibo de electricidad.

3. Seguridad: No todo lo que brilla es GGUF
Hugging Face es el paraíso, pero en 2026 han aparecido archivos maliciosos camuflados como modelos.

Tip: Descarga modelos solo de "creadores" verificados (como Bartowski, MaziyarPanahi o los oficiales de Meta/Google/DeepSeek).

Aislamiento: Como usas CachyOS, considera correr agentes experimentales dentro de un contenedor Docker para que no tengan acceso a tus archivos personales a menos que tú los autorices.

4. El "Corte de Conocimiento" (Knowledge Cutoff)
A diferencia de ChatGPT o Claude web, tu IA local vive en una cápsula del tiempo.

No saben qué pasó hoy: A menos que instales un agente con RAG (Retrieval-Augmented Generation) o búsqueda web (como AnythingLLM), tu modelo no sabrá noticias de última hora de este mes de abril.

Uso ideal: Úsalas para razonar, programar, resumir textos privados o tareas creativas donde la "actualidad" no sea crítica.

5. Mantenimiento del Almacenamiento
Los modelos son adictivos y pesan mucho.

Dieta de modelos: Es fácil terminar con 200GB de modelos que no usas. Acostúmbrate a borrar los que no te den buenos resultados (como hicimos con el 32B).

SSD: Nunca instales modelos en un HDD mecánico. La velocidad de carga del modelo a la RAM/VRAM será un suplicio. En CachyOS, asegúrate de que tu partición sea XFS o Btrfs para un manejo de archivos grandes más eficiente.

6. La IA no es una "Enciclopedia", es un "Motor de Razonamiento"
Mucha gente instala IA local para buscar datos. Error.

Úsala para procesar información que tú le des. Si le pides datos oscuros, va a inventar (alucinar) con mucha seguridad. La IA local brilla cuando tú eres el dueño de los datos y ella es el procesador.

Mi consejo final para tu setup:
Tienes un equipo que está en el "límite generacional". Tu meta no debe ser correr el modelo más grande, sino el más rápido. Un modelo de 8B parámetros respondiendo a 80 tokens por segundo es mucho más útil que uno de 32B que te hace esperar 5 segundos por cada palabra.