Paso 1 — Activar control manual
nvidia-settings -a "[gpu:0]/GPUFanControlState=1"

Paso 2 — Elegir velocidad silenciosa escritorio
nvidia-settings -a "[fan:0]/GPUTargetFanSpeed=20"


Paso 3 — Script modo escritorio
nano ~/gpu-silent.sh

#!/bin/bash
nvidia-settings -a "[gpu:0]/GPUFanControlState=1"
nvidia-settings -a "[fan:0]/GPUTargetFanSpeed=25"
chmod +x ~/gpu-silent.sh

Paso 4 — Script modo gaming
nano ~/gpu-gaming.sh

#!/bin/bash
nvidia-settings -a "[gpu:0]/GPUFanControlState=1"
nvidia-settings -a "[fan:0]/GPUTargetFanSpeed=70"
chmod +x ~/gpu-gaming.sh


Paso 5 — Volver a automático (MUY importante)
nano ~/gpu-auto.sh
#!/bin/bash
nvidia-settings -a "[gpu:0]/GPUFanControlState=0"

