S2I Notas


s2i create danminijavabuilder rootjhttps_dir


podman build --format docker -t danminijavabuilder .



Creo un directorio temporal donde va ir a parar los archivos de la imagen (Dockerfile, updload, download). Esto debido a que no tengo docker instalado y utilizo --as-dockerfile.
En este caso el dir se llama "tempo".
Para ejecutar la siguiente instruccion debo estar parado a nivel test, de la estructura que crea s2i create.

mkdir tempo

[rootjhttps_dir]$ ls -la
drwxr-xr-x. 1 dacelent dacelent  50 Jan 17 18:38 tempo
drwx------. 1 dacelent dacelent  32 Jan 17 18:32 test


[rootjhttps_dir]$ s2i build test/test-app/ localhost/danminijavabuilder borrame:1 --as-dockerfile ./tempo/Dockerfile
WARNING: could not inspect the builder image for labels: pinging container registry localhost: Get "https://localhost/v2/": dial tcp [::1]:443: connect: connection refused
Application dockerfile generated in ./tempo/Dockerfile

- El warning se puede ignorar
- test --> es el directorio donde esta guardada la aplicacion de prueba
- localhost/danminijavabuilder --> es la imagen builder a utilizar. La que fue creada en un paso anterior.
- borrame:1 --> es la nombre:tag de la imagen generada.
- --as-dockerfile --> ./tempo/ puede ser solo el directorio de destino donde van (Dockerfile, updload, download)

cd ..

podman build  -t borrame:1 tempo/
