## NOTE FOR ADVANCED DEPLOYMENT CONFIG 


```bash
oc new-app \
   quay.io/practicalopenshift/hello-world \
   --name advanced-dc \
   --as-deployment-config


oc new-app \
   https://gitlab.com/practical-openshift/hello-world.git \
   --as-deployment-config \
   --name=triggers-demo
```