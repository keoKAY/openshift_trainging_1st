## ServiceAccounts 
> SecurityContextContraints : SCC 

- Control pod permission in openshift 
- Determine pod actions and resource access 


### Default and Custom SCCs 
- Default SCCs created during openshift installations 
- Addtional SCCs can be introduced by installing Operators 
- Create custom SCCs using Openshift CLI  

### BEST practg ice for SCCs 
1. Avoid modifying the default SCCs 
2. Create custom SCCs to suit specific requirements 
3. Use Default SCCs if they meet your needs

### Determing Pod's SCC 
- Pod default to Restricted SCC 
- SCC determined by ServiceAccount , and Groups 
- To find the pod's SCC run under : 
```bash 
oc get pod $POD_NAME -o yaml | grep openshift.io/scc
```
There are three types of SCC: 
- `restricted` (default): 
    - Does not allow root 
    - Does not allow privileged escalation
    - Most secure, default for normal apps 
- `anyuid`: 
    - Allow runnings containers as root ( UID=0 )
    - useful for older apps that require   root privileges  

- `priviledge`
    - Allow full host access (volumn, devices , etc )
    - Only use for system or admin-level pods 
### Demo : Managing the Permission Using Security Context Constraints 
```bash 

oc new-project scc-demo 
oc new-app docker.io/httpd 

# by default, application will run with a restricted security context, which mean it cannot run with the root user 
```
![alt text](image-3.png)

- we will create a service account to resolve this issue and grant to any UID SCC to it 
```bash

oc whoami 
oc add-scc-to-user anyuid -z my-sa 
oc adm policy add-scc-to-user anyuid \                           
    -z nginx-root -n quota-demo

oc apply -f quota-deployment.yaml 

oc create sa my-sa 
# what is anyuid ? allow container to run with any user id , even root 
oc adm policy add-scc-to-user anyuid -z my-sa 

oc patch deploy/quota-deployment \
    --patch '{"spec": {"template": {"spec": {"serviceAccountName": "my-sa" }}}}'

# or alternatively 
oc set sa deployment/quota-deployment my-sa 

oc get -o yaml pod/123 | grep "serviceaccount" 
# my-sa  -> add serviceaccount for containers 
```