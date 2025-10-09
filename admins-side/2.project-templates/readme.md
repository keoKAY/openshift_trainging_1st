


```bash 
# get the template file for customization 
oc adm create-bootstrap-project-template -o yaml > template.yaml 

# go inside the project contains the template.yaml 
oc apply -f template.yaml -n openshift-config 
oc replace -f template.yaml -n openshift-config 

oc get pod -n openshift-apiserver -w 
oc edit projects.config.openshift.io/cluster

```
- You need to paste this snippet for it to work 
```yaml
# add this configurations 
spec:
  projectRequestTemplate:
    name: project-request
```

```bash
# testing 
oc new-project dev-project

oc get quota 
oc get resource quota 

oc get limits 
oc get limitrange

oc new-project tester-project
```