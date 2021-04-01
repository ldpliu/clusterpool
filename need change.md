system:serviceaccount:hive:hive-controllers clusterset add/remove

foundation-sa



kubectl annotate mch multiclusterhub  mch-pause=true



kubectl edit clusterrole open-cluster-management:installer:foundation  set update set/  claim/pool  add to set.

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

