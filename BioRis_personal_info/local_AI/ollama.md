install ollama: 
run ollama: ollama serve (runs server)

sudo systemctl enable ollama
sudo systemctl start ollama
sudo systemctl stop ollama

download models: ollama pull +modelName
example: ollama pull mistral

see local models: ollama list

run model: ollama run +modelName

exit: /bye or ctrl + D

use it as command: ollama run +modelName +"your prompt"
example: ollama run mistral +"hello, how are you?"

with pipes: cat +fileName | ollama run +modelName +"your prompt"

run new model: ollama run +modelName -> every run, runs new model

delete models: ollama rm +modelName
check model size: du -sh ~/.ollama/models -> shows total size of all models


**Summary**
> 1. always install accordingly to hardware specs
    -  check here for details: [text](https://www.canirun.ai/)
    - 