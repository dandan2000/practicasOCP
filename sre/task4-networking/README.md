
oc annotate svc stock service.beta.openshit.io/serving-cert-secret-name: stock-service-cert

oc set probe deploy/stock --readiness --get-url=https://:8085/readyz
oc set probe deploy/stock --liveness --get-url=https://:8085/livez


oc set volume deployment/stock --add --name=v1 -t secret --secret-name stock-service-cert -m /etc/pki/stock/

oc set env deployment/stock TLS_ENABLED=true




oc create cm service-ca
oc annotate cm service-ca service.beta.openshift.io/inject-cabundle="true"

oc set env deployment/product CERT_CA=/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem


Para Product usa un certificado generado por una CA externa

oc create secret tls passthrough-cert --cert certs/product.pem --key certs/product.key

oc set volume deployment/product --add --name=trusted-ca -t configmap --configmap-name service-ca -m /etc/pki/ca-trust/extracted/pem/