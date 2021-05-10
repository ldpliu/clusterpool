system:serviceaccount:hive:hive-controllers clusterset add/remove

foundation-sa



kubectl annotate mch multiclusterhub  mch-pause=true



kubectl edit clusterrole open-cluster-management:installer:foundation  
update set/  
claim/pool  add to set.



kubectl edit clusterrole hive-controllers  ? add to set 

1. delete managedclusterset  clusterrrole (finalizer need change)


webhook config

image controller/webhook







//annottation
project 
gurney

clusterdeployment
claim delete subject

view secret ?

oc get

 labels:
   cluster.open-cluster-management.io/clusterset: server-foundation-clusterset


1. no label describe





kubectl delete clusterrolebinding open-cluster-management:clusterset-admin:server-foundation-clusterset open-cluster-management:clusterset-view:server-foundation-clusterset






in this story, we create two managedclusterset clusterrole when managedclusterset created. and we will do more rbac things based on these two clusterrole in next sprint. curretlly, let me show these two clusterrole.


kubectl get managedclustersets

cat cluster-admin/managedclusterset.yaml

kubectl apply -f cluster-admin/managedclusterset.yaml

kubectl get managedclustersets

kubectl get clusterrole |grep clusterset

kubectl delete -f cluster-admin/managedclusterset.yaml

kubectl get managedclustersets

kubectl get clusterrole |grep clusterset



when we delete the clusterset, the related clusterrole will also be deleted.

And in sprint 3, we define the clusterset admin and cluster view clusterrole,
 In sprint 4, we will deliver more the RBAC detail about clusterset, managedclusters, clusterdeployments and clusterpool based on these two clusterset clusterrole. we will demo it in next sprint playback.
That's all for serverf-foundaiton playback  thank you.


