Construccion de nuevas imagenes de contenedor de aplicaciones con la linea de comando de OCP

oc new-app <INPUT>

Las instrucción soporta estos input:

    - source repo
    - image
    - template
    - binary

Que corresponden con algunos de estos casos de uso principales:

    - Fuentes de la app en repo Git
    - Fuentes de la app en file system local
    - Dockerfile y archivos adicionales en repo Git
    - Dockerfile y archivos adicionales en file system local
    - Imagen de contenedor en un Registry
    - Fuentes y Dockerfile en el mismo repo Git
    - Desplegar un template
    - Agregar un binario executable a una imagen base ya existente 


Fuentes de la app en repo Git

oc new-app https://gitrepo.com/rth
    se puede agregar --source-secret=clave para usar un repo privado
    se puede indicar -- context-dir para el dir de origen del repo
    se puede indicar -- #branch para indicar un branch diferente a master/main

Fuentes de la app en file system local

oc new-app /path

Nota: en el repo local debe haber un remote llamado "origin" que apunte a una URL que sea accesible por OCP. Sino lo hay, se creará un binary build (es decir, se compilan los fuentes en un ejecutable y se arma la imagen con ese archivo).


Un problema inicial que enfrenta este comando es distinguir si la URL proporcionada corresponde a un repositorio GIT o una Registry. La accion por default es considerarlo un repo git y tratar de clonarlo. 
Para solucionar esta ambiguedad OCP  clona localmente y evalua si es código fuente o no.

Podemos utilizar esta opciones para resolver la ambiguedad.

    - --code            indica fuentes
    - --docker-image    indica registry
    - --strategy        opcion adicional
        La opción strategy con una valor entre docker, source, pipeline. Sirve para solucionar problemas de ambiguedad que se dan con solo las opciones code o docker-image. Por ejemplo si tengo en un repo git código fuente junto a un archivo Dockerfile, OCP no sabra que quiero hacer, si compilar los fuente o utilizar el Dockerfile. Con strategy puedo resolver esa ambiguedad usando strategy docker.

Nota: la imagen indicada requiere menos detalles si se utiliza una imagen propia de OCP o de docker.io o una imagen propia que reside en otro registry.

Cuando es código fuente OCP debe decidir como compilar y en base a que imagen base construir la imagen final. Para esto hay dos opciones:

    - ~  

    No requiere cliente Git instalado; ya que no requiere bajar el código para decir que runtime utilizar, se le pasa explicitamente. 
    La tilde indique que no se lleve adelante la detecion de lenguaje de programación del código fuente y que se pasará como parámetro. No indica la imagen de contendor a utilizar sino el runtime o stack tecnologico como PHP, Python o Java. Se puede indicar versión del runtime ej. php:7.0~https://github.com/app
    Esta es la única forma usar una imagen de builder que no sea conocida por OCP.
    OCP buscará entre la imagenes builders que conoce (coincidencia de labels) una que coincida con lo indicado delante de la tilde.

    - -i o --image-stream

    Si requiere cliente Git.
    Directamente se indica la imagen builder de contenedor a utilizar. 

Dockerfile y archivos adicionales en repo Git

    oc new-app <IMAGE> --code <REPOSITORY>
    Se clona el repo para evaluar si la imagen debe ser usada como un builder para el codigo del repo desplegada separadamente (como ej una imagen de un motor de base de datos).

    oc new-app myimagestream/ruby~https://github.com/rth

    oc new-app openshift/ruby:latest~/home/local/app

    oc new-app mysql # from OCP o docker.io

    oc new-app myregistry:5000/example/myimage  #desde un registry privada

    oc new-app my-stream:v1

    oc new-app https://github.com/ruby-app -l name=hello-world

    oc new-app <REPO> --name=myapp
    Sino se define name el nombre se toma desde el nombre del repo o del nombre de la imagen usada. 

Imagen de contenedor en un Registry

oc new-app --docker-image=registry.access.redghat.com/rhel7

Desplegar un template

    oc new-app -f example/template.json --param-file=hello.params -p admin=admin

Otra opciones adicionales

    - --as-deployment-config para que el desplegable sea creado como deployment-config en cambio de deployment que es default.

    - --name es el nombre que usaran los elementos creados

    - -e indica que se pasa un variable de entorno que formará parte de la imagen

    - --build-env indica una variable de entorno para usar en el pod builder.

Los elementos que se crean por medio de la instruccion new-app son 4:

    - deployment
    - service
    - image stream
    - build config

Busqueda de images o templates compatibles:

    oc new-app search php