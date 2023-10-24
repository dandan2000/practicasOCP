Ejecutando contenedores como root utilizando Restricciones de Contexto de Seguridad

Son un mecanismo que se utiliza para definir políticas de seguridad en un clúster de OCP. Estas restricciones permiten controlar las acciones que pueden realizar los pods y los contenedores dentro del clúster. Se pueden especificar quién puede ejecutar un pod con ciertas características y qué acciones pueden realizar los contenedores en términos de acceso a recursos del sistema, permisos de SELinux, capacidades de Linux.

En caso de necesitar ejecutar un contenedor como usuario root se debe realizar una serie de ajustes ya que por default está prohibido.

Para que OCP pueda definir un pod con un usuario fijo (como el 0 root) se debe usar la SCC anyuid. Los SCCs se aplican a los pods en por medio de ServiceAccount (SA). Cuando se crea un pod, OpenShift comprueba si la SA tiene un SCC asociado. Si es así, OpenShift aplica las restricciones del SCC al pod. Si el pod requiere un permiso particular para ejectuarse que la SCC no le brinda, el mismo fallará.

La SA default esta asociada a un SCC restricted, que ignora el userid seteado en el Dockerfile que definió la imagen de contenedor y asigna un numero rando en su lugar.

Para asociar un SA con un SCC diferentes hacer lo siguiente:

1. Crear la SA

    oc create serviceaccount mysa

2. Setear el SCC en la SA

    oc adm policy add-scc-to-user anyiud -z mysa

3. Setear la SA en el recurso de interés

    oc patch cd/demo-app --patch '{"spec:{"template":{"spec":{"ServiceAccountName": "mysa"}}}}


Las SCC incluídas en en cluster son:

restricted: Este SCC es el más restrictivo. Los pods que se ejecutan con este SCC no pueden ejecutar contenedores privilegiados, no pueden escalar privilegios y están limitados a acceder a los recursos del sistema que son necesarios para ejecutar la aplicación.

privileged: Este SCC es el menos restrictivo. Los pods que se ejecutan con este SCC pueden ejecutar contenedores privilegiados, pueden escalar privilegios y tienen acceso a todos los recursos del sistema.

anyuid: Este SCC permite que los pods se ejecuten con cualquier ID de usuario o grupo.

hostnetwork: Este SCC permite que los pods accedan a todos los recursos del sistema host.

hostpid: Este SCC permite que los pods accedan al PID del proceso host.

privileged-scc: Este SCC es similar al SCC privileged, pero permite que los pods accedan a los recursos del sistema que son necesarios para ejecutar el control de acceso basado en roles (RBAC).