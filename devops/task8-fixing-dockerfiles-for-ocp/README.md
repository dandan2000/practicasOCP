Adaptar archivos Dockerfile o containerfile para Openshift

Problemas comunes

    dir y  archivos que son leidos o escritos por los procesos en el contenedor deben tener como dueño al grupo root y tener permisos de eescritura y lectura a nivel grupo. [group = root]

    archivos ejecutables deben tener permisos de ejecución a nivel grupo. [group = exec]

    los procesos ejecutandose en el contenedor no deben escuchar en puertos privilegiados, dado que no se ejecutan con usuario privilegiados. [port < 1024]

    entonces
        si el proceso hace R/W sobre un archivo o dir debe tener permisos RW
        si el proceso ejecuta un archivo este necesita tener permisos RWX
        en ambos casos el grupo debe ser root

agregar a containerfile

    RUN chgrp -R 0 <dir> &&
        chmod -R g=u <dir>

g=u se puede reemplazar por g+rwx con el mismo resultado.

El usuario root tiene permisos especiales, pero el grupo root no, lo que minimiza el riesgo de seguridad.

