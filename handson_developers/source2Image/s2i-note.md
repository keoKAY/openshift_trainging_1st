## Source2Image 

Docker image 
Dockerfile -> Image -> Docker Container


`s2i` -> only need to provide the source code 
    openshift will do the rest 
```bash
oc new-app \
   https://github.com/keoKAY/openshift_developer_materials.git \
   --context-dir s2i/ruby \
   --as-deployment-config \
   --name s2i-demo-1

oc get builds 
oc get bc 
oc get pod 
watch oc get pod

oc new-app https://github.com/keoKAY/reactjs-devop8-template.git#s2i \
   --name=no-dockerfile-app

oc get service 
oc expose service/no-dockerfile-app --port=8080
oc status 
oc get routes 

oc delete pod/s2i-demo-1-1-ld5wm

```

### Explicit Builder Image
```bash

# used specific tags of the builder image
oc new-app nodejs:18-ubi8~https://github.com/keoKAY/reactjs-devop8-template.git#s2i \
   --name=explicit-app-1

oc new-app nodejs~https://github.com/keoKAY/reactjs-devop8-template.git#s2i \
   --name=explicit-app-2

```
### Customize `.s2i/bin/assemble`
```bash
oc new-app \
   https://github.com/keoKAY/openshift_developer_materials.git#s2i-script \
   --context-dir s2i/ruby \
   --name demo-custom-script

oc get build 
oc logs -f builds/demo-custom-script-1 
# look for "Hello from before and after message" 

```
