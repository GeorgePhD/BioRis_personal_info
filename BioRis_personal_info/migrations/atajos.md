# run atajos
## this first try must show errors:
### might need to login to docker first

atajos dicom
ℹ Levantando servicios en /home/george/.local/share/atajos/dicom...
[+] up 2/2
 ✘ Image tothspa/toolkit:latest       Error pull access denied for tothsp...       0.8s
 ! Image tothspa/toolkit-redis:latest Interrupted                                  0.8s
Error response from daemon: pull access denied for tothspa/toolkit, repository does not exist or may require 'docker
 login'

✔ Stack 'dicom' (Toolkit API + Redis) levantado con éxito.
  - API disponible en: http://localhost:9595
  - Redis disponible en: localhost:6379
  - Logs de API en: /home/george/.local/share/atajos/dicom/toolkit/logs
  - Datos de Redis en: /home/george/.local/share/atajos/dicom/toolkit/redis-data

Para ver logs en tiempo real ejecuta:
  atajos dicom logs

~
❯ docker login

USING WEB-BASED LOGIN

i Info → To sign in with credentials on the command line, use 'docker login -u <username>'


Your one-time device confirmation code is: WKNG-GHZQ
Press ENTER to open your browser or submit your device code here: https://login.docker.com/activate

Waiting for authentication in the browser…

WARNING! Your credentials are stored unencrypted in '/home/george/.docker/config.json'.
Configure a credential helper to remove this warning. See
https://docs.docker.com/go/credential-store/

Login Succeeded



## second try success
- atajos dicom


ℹ Levantando servicios en /home/george/.local/share/atajos/dicom...
[+] up 23/23
 ✔ Image tothspa/toolkit-redis:latest Pulled                                                                   11.2s
 ✔ Image tothspa/toolkit:latest       Pulled                                                                   11.2s
 ✔ Network toolkit_net                Created                                                                  0.0s
 ✔ Container toolkit-redis            Healthy                                                                  6.0s
 ✔ Container toolkit-api              Started                                                                  6.0s

✔ Stack 'dicom' (Toolkit API + Redis) levantado con éxito.
  - API disponible en: http://localhost:9595
  - Redis disponible en: localhost:6379
  - Logs de API en: /home/george/.local/share/atajos/dicom/toolkit/logs
  - Datos de Redis en: /home/george/.local/share/atajos/dicom/toolkit/redis-data

Para ver logs en tiempo real ejecuta:
  atajos dicom logs