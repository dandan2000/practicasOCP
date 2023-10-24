Brindar acceso a la Registry Interna de OCP

Cualquier usuario con acceso a OCP puede hacer push y pull de imágenes desde y hacía sus proyectos de acuerdo a su nivel de acceso (permisos sobre esos proyectos).

Rol Admin/edit en un proyecto  ---> puede hacer PULL && PUSH en el proyecto

Rol View en un proyecto        ---> puede hacer PULL


Para los casos que necesitan brindar acceso solo al manejo de la imagenes dentro de un proyecto (como un proyecto shared que almacene imagenes de varios proyectos), existen proyectos especializados:

    registry-viewer & system:image-puller

    Estos roles permiten a los usuarios hacer pul && inspect desde la registry interna.

    registry-editor & system:image-pusher

    Estos roles permiten hacer push & tag de la registry interna.

Los roles system:* son los que menos capacidades otorgan.

Los roles registry:* brindan mayores capacidades de gestión de la registry como por ejemplo crear nuevos proyectos.

    oc policy add-role-to-user system:image-puller username -n ns

Para ejecutar este comando se debe ser administrador del proyecto o del cluster.

En conclusión si sos role:view poder hacer pull. Si sos admin o edit podes hacer pull o push.

Si se necesitan permisos mas exclisivos o puntuales se dispon de system:* o registry:*.

La diferencia es que estos últimos solo dan permisos sobre otras acciones como crear un buildconfig o un deployment.

---

Se define un secreto que almacena las credenciales para que un Deployment pueda acceder a docker.io y descargar la imagen:

oc create secret docker-registry mysecret --docker-server=docker.io --docker-username=XXX --docker-password=XXX --docker-email=miemail@gmail.com

oc secret link default mysecret --for=pull

Hay un tipo de secreto especial para almacenar credenciales de Regitry. 
Se enlaza el secreto mysecret a la ServiceAccount default (que es la que default de los pods) para que pueda tomar la info desde ahi para auteticarse frente a la Registry.
