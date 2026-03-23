./findscu_v2.sh date 20220101-20221231 MMRAD_HISTORICO_RANCAGUA > rancagua_mmrad_2022.txt

./echo.sh MMRAD_HISTORICO_RANCAGUA

nano nodes.ini

screen -S rancagua_mmrad

cd dicom_v2/

| jq = program to process json files


cat rancagua_mmrad_2022.txt | jq -r '.[] | .StudyInstanceUID'

cat /tmp/faltantes_rancagua_mmrad_2022.no_existen | parallel --jobs 30 --bar --progress ./movescu.sh {} MMRAD_HISTORICO_RANCAGUA CUIDATE


atajos readsalud.arc



#!/bin/bash
fileTotal=$1
fileExist=$2
fileNoExist=$3

if [[ -z $fileTotal ]];then
    echo "Falta el archivo fileTotal"
    exit
fi

if [[ -z $fileExist ]];then
    echo "Falta el archivo fileExist"
    exit
fi

if [[ -z $fileNoExist ]];then
    echo "Falta el archivo fileNoExist"
    exit
fi