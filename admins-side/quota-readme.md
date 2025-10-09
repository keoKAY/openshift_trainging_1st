## NOTE 

### 1. QUOTA 
1. resource quota 
2. project quota 
3. limitrange 
4. project template 


### 1.1 Resource Quota 
Ex. Specify number of pods , cpu, memory for the project to use 

```bash 
oc new-project quota-demo 

oc get quota

oc create quota custom-quota \
    --hard=cpu=2,memory=10Gi,pods=5

# 2 is 2 cores 
oc get quota
oc describe quota custom-quota 
oc run nginx --image=bitnami/nginx # should show error message 
#oc run earthdx --image=quay.io/keokay888/earthdx-private


oc apply -f <file-name.yaml> 
oc scale deployment/quota-deployment --replicas=5
# if we scale it to 8
oc scale deployment/quota-deployment --replicas=8

oc events 
oc get events 

```


### 1.2 Project Quota ( ClusterResourceQuota )
We have three projects 
1.`team-a-dev`
2.`team-a-prod`
3.`team-a-test`

> We want to set the quota for team-a , to have max of 10pods , 2cpus and 2Gi of RAM

```bash
oc get clusterresourcequota 

# 1. Create three project 
oc new-project team-a-dev
oc new-project team-a-test
oc new-project team-a-prod

# 2. Annotation ( Labeling ) three project for apply quota 
oc annotate namespace team-a-dev openshift.io/requester=team-a --overwrite
oc annotate namespace team-a-test openshift.io/requester=team-a --overwrite
oc annotate namespace team-a-prod openshift.io/requester=team-a --overwrite

# apply -f team-a-quota.yaml
oc apply -f team-a-quota.yaml

oc scale deploy/quota-deployment --replicas=10

oc delete clusterresourcequota team-a-quota
oc delete project team-a-dev team-a-test team-a-prod
```
### 1.3 LimitRange 
> Detemrine the resources  of pod containers 
- max/min resource allocation per pod or container 

```bash 
oc new-project limit-test

oc apply -f limitrange-demo.yaml
oc apply -f pod-test.yaml
oc apply -f pod-complied.yaml 
```

### 1.4 Project Templates

```bash 
o oc adm create-bootstrap-project-template -o yaml > template.yaml
oc replace -f template.yaml -n openshift-config 

```