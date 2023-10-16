oc whoami -t
podman login --tls-verify=false https://default-route-openshift-image-registry.apps.kali.rlab.sh -p token -u user

It is not needed to modify this file to login
- cat ~/.config/containers/registries.conf

helm create myapache
helm install run1 myapache
helm upgrade run1 myapache
helm uninstall run1


- Solo con la siguiente instruccion funciona, coloca los parametros en valores default:

oc set probe deployment/run1-myapache --readiness --readiness --get-url=https://:8443/

          readinessProbe:
            httpGet:
              path: /
              port: 8443
              scheme: HTTPS
            timeoutSeconds: 1
            periodSeconds: 10
            successThreshold: 1
            failureThreshold: 3
          livenessProbe:
            httpGet:
              path: /
              port: 8443
              scheme: HTTPS
            timeoutSeconds: 1
            periodSeconds: 10
            successThreshold: 1
            failureThreshold: 3
          readinessProbe:
            httpGet:
              path: /
              port: 8443
              scheme: HTTPS
            timeoutSeconds: 1
            periodSeconds: 10
            successThreshold: 1
            failureThreshold: 3

Se elimina con:

oc set probe --remove --liveness --readiness deployment/run1-myapache

oc set probe deployment/run1-myapache --liveness -- /home/script.sh 

oc set probe deployment/run1-myapache --liveness --open-tcp=8443
