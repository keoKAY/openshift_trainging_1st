## WEBHOOK 

```bash
oc new-build https://github.com/keoKAY/reactjs-devop8-template.git \
   --strategy=docker --name=earthdx-build

oc get build 
oc get bc # buildconfig 
oc logs -f builds/earthdx-build-1
oc start-build earthdx-build # start another build 

# To start application 
oc new-app earthdx-build --name=reactjs-build
# Expose service 
oc expose service/reactjs-build --port=8080


oc get bc 
oc describe bc/earthdx-build 

oc get -o yaml bc/earthdx-build

export GENERIC_SECRET=tjI2gFse9MVfLxyEqVEX
echo $GENERIC_SECRET

TOKEN=$(oc whoami -t)
curl -X POST -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" -k \
    "https://api.crc.testing:6443/apis/build.openshift.io/v1/namespaces/advanced-dc/buildconfigs/earthdx-build/webhooks/$GENERIC_SECRET/generic"
```