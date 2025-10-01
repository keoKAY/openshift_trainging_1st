## NOTE 
### 1. BuildHook for BuildConfig 
```bash
oc new-project buildhook-demo 


oc new-build \
    https://github.com/keoKAY/openshift_developer_materials.git \
    --context-dir hello-world-go --name=hello-world

oc get build 
oc get bc 

# to see the logs of hello-world-1 build 
oc logs -f builds/hello-world-1

# set the buildhook
oc set build-hook bc/hello-world \
   --post-commit \
   --script="echo Hello from build hook"

# to set the command hooks 
oc set build-hook bc/hello-world \
    --post-commit --command -- ./run-tests.sh

# to remove the hooks 
oc set build-hook bc/hello-world \
    --post-commit --remove


# To actually see the result of the builds and build config 
oc get bc 
oc new-app hello-world 
# pod -> used the image with specific tag to run 
oc expose service/hello-world --port=8080

curl url # oc status 
```