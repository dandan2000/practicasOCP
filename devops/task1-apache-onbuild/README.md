This is a simple practice about ONBUILD docker instruction.
Note: ONBUILD doesn't exist in oci format, because we added the --format flag.

cd parent
podman build --format docker -t apache-parent .

podman run --name apache-parent --rm -p 8443:8443  localhost/apache-parent:latest

curl -k https://localhost:8443/index.html

We are going to get "404 Not Found", cause the page isn't within the container.

cd ..
cd child

podman build --format docker -t apache-child .

podman run --name apache-child --rm -p 8443:8443  localhost/apache-child:latest

curl -k https://localhost:8443/index.html

We are going to get a greeting page cause this was added during the build process of the child container.