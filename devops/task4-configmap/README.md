Make a configmap objetc and mount it on a deployment
NOTA: configmap max length is 1 Megabyte

# Se setea una variable de entorno en el deployment por medio de un configmap

oc create configmap mycm --from-literal clave=valor

oc set env --from=configmap/mycm deployment/run1-myapache

# Parece que el nombre de la variable se forma desde el UPPERCASE del nombre de la clave.
echo $CLAVE

# Se setea una variable de entorno en el deployment directamente sin usar configmap

oc set env deployment/run1-myapache lavariable=elvalor

echo $lavariable

# Se remueve la variable de entorno en el deployment, clave y guion seguido.

oc set env deployment/run1-myapache VALOR-

# Se setea una variable de entorno en el deployment desde un archivo. Todo el contenido en la variable.
NOTA: Y tmb datos binarios con binaryData:

oc create configmap game-config-2  --from-file=tempo.yaml

# Se setea una variable de entorno en el deployment desde un directorio. El cm tiene varias clave y se setea de a una en una variable.

oc create configmap game-config-3  --from-file=tempdock/

# Se listan variables de entorno

oc set env deployment/run1-myapache --list

# Se setea configmap en deployment en un container partiular y un prefijo a las claves

oc set env deployment/run1-myapache --from=configmap/mycm -c="myapache" --prefix=pre

oc set env deployment/run1-myapache --list
# deployments/run1-myapache, container myapache
enlinea=enter
# CLAVE from configmap mycm, key clave
# preCLAVE from configmap mycm, key clave

# Otro caso de uso es montar un configmap como un volumen en un path especifico
No encuentro forma de hacerlo dede la linea de comandos.

spec:
  containers:
    - name: test-container
      image: gcr.io/google_containers/busybox
      command: [ "/bin/sh", "cat", "/etc/config/special.how" ]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: special-config