Working with Templates

From previous generated artifacts in Task 2:

    - generate a template producing the same result.
    - replace hardcode values with variables (2 type)
    - impact the template from local filesystem and from a namespace

Highlight Notes:

    1. name used in variable must be form from letters and numbers plus _ symbol.

    2. As in bash variables names in the template are enclosed by ${VAR} for string and ${{VAR}} por numbers and others cases.

    3. One example could be taken from openshift namespace, oc get template -n openshift; oc get template xxx -o yaml -n openshift

    4. To process the template use:
        oc process -f template-myapache.yaml -p SERVICE_PORT=8443 |oc create -f -

    5. The template header is:
        apiVersion: template.openshift.io/v1
        kind: Template
        metadata:
        name: myapache
        objects:
        # and the one line like this for object
        - apiVersion: apps/v1

    6. Crear los recursos con una etiqueta particular:

        oc process -f template-myapache.yaml -p SERVICE_PORT=8443 -l name=practicando |oc create -f -
        oc get all -l name=practicando
        oc delete all -l name=practicando
    
    7. Listar los parametros seteables de un template:

       oc process --parameters  template/cache-service -n openshift

       oc process --parameters -f template-myapache.yaml

    8. Parametros auto-generados, por ejemplo una passwor de 12 caracteres

        parameters:
        - name: PASSWORD
            description: "The random user password"
            generate: expression
            from: "[a-zA-Z0-9]{12}"



