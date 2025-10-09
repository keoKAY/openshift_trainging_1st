## NOTE for workign with user and groups

### 1. WOrking with Users
```bash
sudo apt install apache2-utils -y 
oc login -u kubeadmin # login as cluster admin 


htpasswd -cBb htpasswd alice redhat123
htpasswd -Bb htpasswd secondadmin redhat123
htpasswd -Bb htpasswd bob redhat123
htpasswd -Bb htpasswd james redhat123


# create secret for new user 
oc create secret generic my-htpass-secret \
   --from-file=htpasswd=htpasswd \
   -n openshift-config

oc get secret -n openshift-config 


# original name is htpass-secret
oc get oauth cluster -o yaml > oauth.yaml

# edit the htpasswd spec to use the file called my-htpass-secret 
oc replace -f oauth.yaml 

oc explain oauth.spec
oc explain oauth.spec.identityProviders

oc get pod -n openshift-authentication -w

oc login -u alice -p redhat123
oc login -u james -p redhat123
```

### 2.REMOVE users from file 
```bash 

# replace the generic secret file 
oc create secret generic my-htpass-secret --from-file=htpasswd --dry-run=client \
   -o yaml -n openshift-config \
   | oc replace -f -

oc get users 
oc delete user alice
oc get identity 

oc delete identity developer:alice
```


### 3.UPDATE PASSWORD 
```bash 
# new pass : 123james
htpasswd -bB  htpasswd james 123james
htpasswd -bB  htpasswd bob 123bob

# update the secret object 
oc create secret generic my-htpass-secret --from-file=htpasswd --dry-run=client \
   -o yaml -n openshift-config \
   | oc replace -f -

oc login -u bob -p 123bob
```

## 2. Working with groups 

```bash 

oc adm groups new developers 
oc adm groups add-users developers bob
oc adm groups add-users developers secondadmin
oc adm groups remove-users developers secondadmin

oc edit groups developers

# add view role for the to be able to view the project called myproject 
oc adm policy add-role-to-group \
   view developers -n myproject

oc get groups 
oc adm policy who-can create project 

crc console --credentials 
```
## ROLE BASE ACCESS CONTROL 

```bash
# 1. adding role within a specific project
oc adm policy add-role-to-user self-provisioner james \
    -n myproject

# admin is project admin 
oc adm policy add-role-to-user admin james \
    -n myproject

# remove role from user within in a project
oc adm policy remove-role-from-user admin james -n myproject
# add role to a group within a project
oc adm policy add-role-to-group cluster-admin developers 
oc adm policy remove-role-from-group cluster-admin developers 



oc adm policy add-role-to-group cluster-admin system:authenticated:oauth


oc adm policy add-role-to-user cluster-admin james 
# remove a role a group within a project
oc adm policy remove-role-from-group <role> <group> -n <project>
# list users/groups with a specific permissions
oc adm policy who-can <verb> <resource>

oc adm policy who-can get groups

#oc adm policy can-i create group
```
> when adding cluster-admin , doesn't work directly like project admin 
