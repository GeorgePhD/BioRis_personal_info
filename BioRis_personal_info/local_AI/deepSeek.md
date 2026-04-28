# 🧠 Guía de Configuración: DeepSeek-R1 en CachyOS
> **Estado:** Optimizado para Abril 2026  
> **Hardware Target:** i7-9th Gen | 48GB RAM | RTX 3070 Ti (8GB VRAM)
> **deepseek-r1:8b** (most suitable for this hardware)
---

## 💻 Especificaciones del Sistema
| Componente | Detalle |
| :--- | :--- |
| **CPU** | Intel Core i7-9700 (8 Cores / 8 Threads) |
| **RAM** | 48 GB DDR4 |
| **GPU** | NVIDIA GeForce RTX 3070 Ti (8 GB VRAM) |
| **OS** | CachyOS (Kernel optimizado x86-64-v3) |
| **Backend** | Ollama AI Engine |

---

## 🛠️ 1. Preparación del Modelo Base
Para equilibrar el razonamiento profundo y tu capacidad de RAM, utilizamos la versión **Distill 32B**.

```bash
# Descargar el peso del modelo base
ollama pull deepseek-r1:32b

📝 2. Creación del archivo de configuración (Modelfile)
Crea un archivo de texto plano llamado deepseek-custom.mf. Esta configuración realiza un offloading inteligente: carga lo máximo posible en los 8GB de tu GPU y el excedente en los 48GB de RAM.

# Nombre del archivo: deepseek-custom.mf
FROM deepseek-r1:32b (older, uninstalled due to size) / > **deepseek-r1:8b** (most suitable for this hardware)

# --- CONFIGURACIÓN DE HARDWARE ---
# Cargamos 12 capas en la GPU (~6.5GB VRAM). 
# Esto evita el desbordamiento de memoria en la 3070 Ti.
PARAMETER num_gpu 12

# Ajuste exacto para los 8 núcleos del i7-9th Gen.
PARAMETER num_thread 8

# Ventana de contexto (24k tokens) para análisis de documentos.
PARAMETER num_ctx 24576

# --- PARÁMETROS DE IA ---
# Temperatura baja para respuestas técnicas precisas.
PARAMETER temperature 0.6

# --- PROMPT DE SISTEMA ---
SYSTEM """
Eres DeepSeek-R1, un modelo de razonamiento avanzado ejecutándose en CachyOS.
Tu hardware local incluye una RTX 3070 Ti y 48GB de RAM.
Procesa tus pensamientos de forma lógica y muéstralos siempre dentro de etiquetas <|thought|>.
"""

🚀 3. Despliegue del Modelo
Ejecuta los siguientes comandos en tu terminal de CachyOS:

A. Compilar la instancia optimizada
ollama create ds-pro -f deepseek-custom.mf

B. Iniciar ejecución
ollama run ds-pro

Aquí tienes el contenido optimizado con sintaxis Markdown avanzada, utilizando iconos, bloques de código resaltados y una estructura limpia para que se vea profesional en cualquier lector de Markdown (como Obsidian, VS Code, GitHub o Notion).

Markdown
# 🧠 Guía de Configuración: DeepSeek-R1 en CachyOS
> **Estado:** Optimizado para Abril 2026  
> **Hardware Target:** i7-9th Gen | 48GB RAM | RTX 3070 Ti (8GB VRAM)

---

## 💻 Especificaciones del Sistema
| Componente | Detalle |
| :--- | :--- |
| **CPU** | Intel Core i7-9700 (8 Cores / 8 Threads) |
| **RAM** | 48 GB DDR4 |
| **GPU** | NVIDIA GeForce RTX 3070 Ti (8 GB VRAM) |
| **OS** | CachyOS (Kernel optimizado x86-64-v3) |
| **Backend** | Ollama AI Engine |

---

## 🛠️ 1. Preparación del Modelo Base
Para equilibrar el razonamiento profundo y tu capacidad de RAM, utilizamos la versión **Distill 32B**.

```bash
# Descargar el peso del modelo base
ollama pull deepseek-r1:32b
📝 2. Creación del archivo de configuración (Modelfile)
Crea un archivo de texto plano llamado deepseek-custom.mf. Esta configuración realiza un offloading inteligente: carga lo máximo posible en los 8GB de tu GPU y el excedente en los 48GB de RAM.

Dockerfile
# Nombre del archivo: deepseek-custom.mf
FROM deepseek-r1:32b

# --- CONFIGURACIÓN DE HARDWARE ---
# Cargamos 12 capas en la GPU (~6.5GB VRAM). 
# Esto evita el desbordamiento de memoria en la 3070 Ti.
PARAMETER num_gpu 12

# Ajuste exacto para los 8 núcleos del i7-9th Gen.
PARAMETER num_thread 8

# Ventana de contexto (24k tokens) para análisis de documentos.
PARAMETER num_ctx 24576

# --- PARÁMETROS DE IA ---
# Temperatura baja para respuestas técnicas precisas.
PARAMETER temperature 0.6

# --- PROMPT DE SISTEMA ---
SYSTEM """
Eres DeepSeek-R1, un modelo de razonamiento avanzado ejecutándose en CachyOS.
Tu hardware local incluye una RTX 3070 Ti y 48GB de RAM.
Procesa tus pensamientos de forma lógica y muéstralos siempre dentro de etiquetas <|thought|>.
"""
🚀 3. Despliegue del Modelo
Ejecuta los siguientes comandos en tu terminal de CachyOS:

A. Compilar la instancia optimizada
Bash
ollama create ds-pro -f deepseek-custom.mf
B. Iniciar ejecución
Bash
ollama run ds-pro
📈 Monitoreo de Recursos
Para verificar que el balanceo entre CPU/GPU es correcto, mantén estas herramientas abiertas:

Terminal 1 (GPU): nvtop

(Verás la VRAM estabilizada en ~7GB).

Terminal 2 (CPU/RAM): btop

(Verás un uso intensivo de RAM de aproximadamente 20GB).

💡 Tips de Rendimiento para CachyOS
Prioridad del Proceso: Para evitar latencia en el escritorio mientras el modelo piensa:

sudo renice -n -10 -p $(pgrep ollama)

Hugepages: Mejora la velocidad de acceso a la RAM:

echo always | sudo tee /sys/kernel/mm/transparent_hugepage/enabled

Guía generada para entorno local privado - 2026



## best settings

Here’s the content you can save in a file named `deepseek_settings.md`:

```markdown
# DeepSeek Settings - Optimized for Speed and Precision

This file contains the best configuration settings for DeepSeek to ensure fast and precise responses.

---

## Model Settings

### Temperature
Temperature controls the randomness of the model's output. Lower values make the model more deterministic and
focused, while higher values increase creativity.

```yaml
temperature: 0.2  # Low value for more focused and precise responses
```

### Top-p (Nucleus Sampling)
Top-p sampling selects from the top `p` percentage of most likely tokens. This can help balance creativity
and precision.

```yaml
top_p: 0.8  # 80% of the most likely tokens
```

### Top-k
Top-k sampling selects from the top k most likely tokens at each step.

```yaml
top_k: 50  # Top 50 tokens
```

### Max Tokens
The maximum number of tokens to generate in the response.

```yaml
max_tokens: 4096  # Adjust based on your needs
```

### Stop Sequences
Stop sequences halt the response when one of the specified sequences is encountered.

```yaml
stop: ["\n", "###", "```"]  # Common stop sequences for code/math responses
```

---

## Response Format

### Response Format
Enforce a specific response format for structured outputs.

```yaml
response_format: "json"  # or "markdown", "text", etc.
```

### Format Guidelines
Provide guidelines for the model to follow specific formatting rules.

```yaml
format_guidelines: |
  - Use bullet points for lists.
  - Use headings for sections.
  - Bold important terms.
```

---

## Advanced Settings

### Stop Tokens
Stop tokens are specific token sequences that stop the response.

```yaml
stop_tokens: ["<|endoftext|>", "</s>", "###"]
```

### Presence Penalty
Penalty for repeating the same token. Positive values discourage repetition.

```yaml
presence_penalty: 0.0  # Disable repetition penalty
```

### Frequency Penalty
Penalty for repeating the same token. Positive values discourage repetition.

```yaml
frequency_penalty: 0.0  # Disable frequency penalty
```

### Seed
For deterministic results, you can set a seed for reproducibility.

```yaml
seed: 1234567890
```

---
> **deepseek-r1:8b** (most suitable for this hardware)
## Example Configuration File

```yaml
model: deepseek-coder
temperature: 0.2
top_p: 0.8
top_k: 50
max_tokens: 4096
stop: ["\n", "###", "```"]
response_format: "json"
presence_penalty: 0.0
frequency_penalty: 0.0
seed: 1234567890
```

---

## Notes
> **deepseek-r1:8b** (most suitable for this hardware)
- These settings are optimized for fast and precise responses.
- Adjust the settings based on your specific use case (e.g., creative writing vs. technical documentation).
- Some settings might not be available in all models or platforms.

You can adjust the values as needed and save this file in the `~` directory (your home directory) with the
name `deepseek_settings.md`.
```

To create this file, you can:

1. Open a text editor (like Notepad, VS Code, TextEdit, etc.)
2. Copy and paste the content above
3. Save the file as `deepseek_settings.md` in your home directory

On Linux or macOS, you can also use the terminal:

```bash
touch ~/deepseek_settings.md
```
> **deepseek-r1:8b** (most suitable for this hardware)
Then open the file in a text editor and paste the content.