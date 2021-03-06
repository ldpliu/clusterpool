## Demo:
I will demo the rbac prototype about clusterset managedclusters and clusterpool
we define three Persona: cluster-admin(gurney) team-admin(le jian) team-user(dangpeng)
we use gurney as cluster-admin, le and jian is team-admin, dangpeng is team-user



## Cluster-admin Create clusterset
oc adm policy add-cluster-role-to-user cluster-admin gurney

cat cluster-admin/managedclusterset.yaml
oc create -f cluster-admin/managedclusterset.yaml --as gurney

[Note] after the clusterset created, we will create two clusterrole, clusterset admin, clusterset view

kubectl get clusterrole| grep set

## Cluster-admin Grant permission to team
oc adm policy add-cluster-role-to-user open-cluster-management:managedclusterset:admin:server-foundation-clusterset le jian --as gurney
oc adm policy add-cluster-role-to-user open-cluster-management:managedclusterset:view:server-foundation-clusterset dangpeng --as gurney

##//oc adm policy add-cluster-role-to-user self-provisioner le jian --as gurney


## team-admin create cluster
[Note] Then for team-admin, if he want to create managedcluster, he must set the clusterset label. if not,  the create request will be denied. lets try to create a managedcluster which has no set label. and it will fail

cat team-admin/managedcluster-nolabel.yaml
oc create -f team-admin/managedcluster-nolabel.yaml --as le

cat team-admin/managedcluster-set-donot-exist.yaml
kubectl create -f team-admin/managedcluster-set-donot-exist.yaml --as le

cat team-admin/managedcluster-label.yaml
oc create -f team-admin/managedcluster-label.yaml --as le

## team-admin create clusterdeployment
[Note]after the managedcluster created, then team-admin need to create a clusterdeployment to provision a  cluster, same as managedlcuster, the clusterdeployment should also have clusterset label. 

cat team-admin/clusterdeployment-nolabel.yaml
oc create -f team-admin/clusterdeployment-nolabel.yaml --as le

cat team-admin/clusterdeployment-label.yaml
oc create -f team-admin/clusterdeployment-label.yaml --as le





######################################## view
### another team-admin edit managedcluster
oc get managedclusters.clusterview --as jian
oc annotate managedclusters managedcluster1 testupdate1=l1 --as jian


oc get managedclusters.clusterview --as dangpeng
oc annotate managedclusters managedcluster1 testupdate2=l1 --as dangpeng


oc get project --as jian
oc get project --as dangpeng
oc get clusterdeployment -n managedcluster1 --as jian
oc get clusterdeployment -n managedcluster1 --as dangpeng

oc annotate clusterdeployment managedcluster1 -n managedcluster1 a=b --as jian
oc annotate clusterdeployment managedcluster1 -n managedcluster1 a1=b --as dangpeng



In this scario, we create a mangaecluster and use clusterdeployment to provision a managedcluster.

Then we will demo to use clusterpool and clusterclaim to provision clusters.
Firstlly, gurney need to create a project for clusterpool and clusterclaim.



# s2
------------
## Cluster-admin Create clusterset

cat cluster-admin/managedclusterset.yaml
oc create -f cluster-admin/managedclusterset.yaml

## Cluster-admin Grant permission to team
oc adm policy add-cluster-role-to-user open-cluster-management:managedclusterset:admin:server-foundation-clusterset le jian
oc adm policy add-cluster-role-to-user open-cluster-management:managedclusterset:view:server-foundation-clusterset dangpeng

-----------------


## Cluster-admin create ns and grant permission
oc adm new-project server-foundation-clusterpool --as gurney
oc adm policy add-role-to-user admin  --namespace  server-foundation-clusterpool le jian --as gurney

[note]cluster-admin also can give self-provisioner permission to team-admin, then team-admin can create the project by themselves




## team-admin create Clusterpool 
oc create secret generic server-foundation-aws-creds -n server-foundation-clusterpool --from-literal=aws_access_key_id=${AWS_ACCESS_KEY_ID} --from-literal=aws_secret_access_key=${AWS_SECRET_ACCESS_KEY} --as le
oc create secret generic server-foundation-ocp-pull-secret --from-file=.dockerconfigjson=team-admin/clusterpool/pull-secret.txt --type=kubernetes.io/dockerconfigjson -n server-foundation-clusterpool --as le


cat team-admin/clusterpool/clusterpool-nolabel.yaml
oc create -f team-admin/clusterpool/clusterpool-nolabel.yaml --as le

cat team-admin/clusterpool/clusterpool-label.yaml
oc create -f team-admin/clusterpool/clusterpool-label.yaml --as le


oc get clusterdeployment --all-namespaces

## team-admin create Claim
cat team-admin/clusterpool/clustercliam.yaml
oc create -f team-admin/clusterpool/clustercliam.yaml --as le


[Note] after clusterclaim and clusterdeployment created, we will add the clusterset lable to clusterclaim and clusterdeployment, and claim and clusterdeployment will be added to this set. so clusterset admin and clusterset view will has permission to access them.

Then let's use other team member to access the clusterpool





## another team-admin get clusterdeployment
oc get clusterpool -n server-foundation-clusterpool --as jian
oc get clusterpool -n server-foundation-clusterpool --as dangpeng
oc label clusterpool -n server-foundation-clusterpool  ocp46-aws-clusterpool testlabel=a1 --as jian
oc label clusterpool -n server-foundation-clusterpool  ocp46-aws-clusterpool testlabel1=a2 --as dangpeng

oc get clusterclaim.hive.openshift.io -n server-foundation-clusterpool --as jian
oc get clusterclaim.hive.openshift.io -n server-foundation-clusterpool --as dangpeng

oc get project --as jian
oc get project --as dangpeng

oc get clusterdeployment --all-namespaces

oc get clusterdeployment 


set delete/ delete resource ?





kubectl delete -f cluster-admin/managedclusterset.yaml
kubectl delete -f team-admin/managedcluster-label.yaml
kubectl delete clusterrolebinding open-cluster-management:managedclusterset:admin:server-foundation-clusterset open-cluster-management:managedclusterset:view:server-foundation-clusterset

kubectl delete -f team-admin/clusterpool
kubectl delete -f team-admin/

oc delete ns server-foundation-clusterpool

kubectl delete clusterdeployment managedcluster1 -n managedcluster1




???
delete rolebinding first, then delete set??




kustomize build deploy/foundation/klusterlet | kubectl delete -f -
kustomize build deploy/foundation/hub | kubectl delete -f -


make deploy-foundation-hub GO_REQUIRED_MIN_VERSION:=
make deploy-foundation-agent GO_REQUIRED_MIN_VERSION:=



kubectl get managedclusters 
kubectl get managedclustersets

kubectl get rolebinding -n cluster1

kubectl get clusterrolebinding |grep set





cluster.open-cluster-management.io/clusterset: clusterset1
cluster.open-cluster-management.io/clusterset: server-foundation-clusterset

clusterset1

open-cluster-management:managedclusterset:admin:managedcluster:cluster1


export KIND_CLUSTER=rbac
cp ~/.kube/config .kubeconfig

// add in klusterlet/install.sh
cp .kubeconfig registration-operator/
export KUBECONFIG=/root/.kube/config





Q:
1. le in group server-foundation, and create clusterrolebding, but le can not list clusterview
