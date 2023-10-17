Metricas

Desde:

node_exporter
oc exec -n openshift-monitoring -i -t -c node-exporter $(oc get pod -l "app.kubernetes.io/name=node-exporter" -o jsonpath='{.items[0].metadata.name}' -n openshift-monitoring) -- curl 127.0.0.1:9100/metrics

kube-state-metrics
oc exec -n openshift-monitoring -i -t -c kube-state-metrics $(oc get pod -l "app.kubernetes.io/name=kube-state-metrics" -o jsonpath='{.items[0].metadata.name}' -n openshift-monitoring) -- curl 127.0.0.1:8081/metrics

openshift-state-metrics
oc exec -n openshift-monitoring -i -t -c openshift-state-metrics $(oc get pod -l 'app.kubernetes.io/name=openshift-state-metrics' -o name -n openshift-monitoring) -- curl 127.0.0.1:8082/metrics


Listar todos los contenedores con restart, excluyendo los namespace que son de openshift

kube_pod_container_status_restarts_total{namespace!~"openshift.+",namespace!~"kube.+"}