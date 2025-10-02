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


# export from cache dir 
export KUBECONFIG=~/.crc/machines/crc/kubeconfig


# ConfigChange Triggers 
oc new-project trigger-demo 

oc new-app \
   quay.io/practicalopenshift/hello-world \
   --name advanced-dc \
   --as-deployment-config

oc get dc 
oc describe dc/advanced-dc 

oc rollout latest dc/advanced-dc 

# run this , to change the configuration of the dc 
oc set volume dc/advanced-dc \
   --add \
   --type emptyDir \
   --mount-path /config-change-demo


# to view our triggers 
oc set triggers dc/advanced-dc 
# to remove triggers 
oc set triggers dc/advanced-dc \
   --remove --from-config 

# to add the configchange trigger 
oc set triggers dc/advanced-dc --from-config 


# ImageChange Trigger 
oc set triggers dc/advanced-dc --remove \
   --from-image advanced-dc:latest

oc set triggers dc/advanced-dc \
   --from-image advanced-dc:latest \
   -c advanced-dc

```


## Deployment Strategies 

Deployment Hook 

```bash 
oc new-app \
   quay.io/practicalopenshift/hello-world \
   --as-deployment-config

oc rollout latest dc/hello-world

oc set deployment-hook dc/hello-world \
    --pre \
    -c hello-world \
    -- /bin/echo Hello from pre-deploy hook

# to see event that happend recently in openshift cluster. 
oc events 
oc describe dc/hello-world # Strategy: Recreate


# to open the editor as nano instead of vim when run oc edit 
export EDITOR=nano


```

#### To update the config in openshift 
1. get yaml file , update yaml , `oc replace -f file.yaml`
2. `oc edit dc/hello-world` change the strategy to `type: Recreate`
->  `i` -> insert mode 
-> `/strategy + Enter` -> search 
->  `ESC + :wq!` -> to save the changes 
