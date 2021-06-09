# RBAC model around ManagedClusterset in Red Hat Advanced Cluster Management for Kubernetes

In this blog, we introduce the RBAC model around managedclusterset in Red Hat Advanced Cluster Management for Kubernetes that is availabel in version 2.3. 

[Red Hat Advanced Cluster Management for Kubernetes](https://www.redhat.com/en/technologies/management/advanced-cluster-management) provides end-to-end visibility and control to manage your Kubernetes clusters, and controls your application lifecycle across the hybrid clouds. There are a couple of resources defined in Red Hat Advanced Cluster Management for Kubernetes, like managedclusterset, managedcluster, clusterpool, clusterclaim, clusterdeployment.

some resources has direct relationship, but the permissions could be segregate. it will cause confuse.
currentlly, when a managedcluster created, we will auto create a cluster namespace, many resources can be created in this ns. for a user, he can be grant permission to the mangedcluster and ns separatelly. it will cause confuse for each model.

same as clusterpool, clustercalim, clusterdeployment.
if clusteradmin want to give permission to these resources, he must create rolebinding to these resources one by one. it is complex.

So we use managedclusterset to solve this problem. 


## Managedclusterset:
ManagedClusterSet resources allow the grouping of cluster related resources, which enables role-based access control management across all of the resources in the group.

the following resources can be add to set by set label "a=b".
|  Resource Type   |  ResourceGroup   |   Description  |
|  ----  | ----  |  --- |
| ManagedClusterset  |  | we define it as a resource group. administrator can set permission to it. 
| ManagedCluster  |  | can be add to managedclusterset 
| Resources in ManagedCluster Namespace ||
| ClusterDeployment||
| Clusterpool||
| ClusterClaim||

ManagedClusters/clusterdeployment... can be add to managedclusterset by set label:...


When clusterset created, we will auto create two clusterrole for this clusterset, one is clusterset admin, another is view.

ClusterRole:
clusterset-admin
clusterset-view
 
user could have clusterset admin or view permission by bind admin or view role. with this role, user will also have admin/view permission to resources which list in up tables wich has this clusterset label. 

# expect action
we define three role for env. and list some expect action on them.


## cluster-admin
has all administrator permissions for OCM resources. 
1.  should create clusterset for each team, 
2. give team-admin and team-view permissions for this set. 
3. give self-provisioner permission to team admin, so he can provison cluster by himself.

## As team-admin
1. could create project, 
2. could create a new cluster in cloud providers, or import an existing cluster.
3. could create a clusterpool and claim the cluster in this pool
4. could access the clusterpool's cluster.
5. could get/update/delete all clusters/clusterpool/clustercliams in the set.

## As team-view
1. could get all clusters/clusterpool/clustercliams in the set.
2. could not update/delete/ clusters/clusterpool/clustercliams in the set.



# Best practice:
scenarios: 

cluster-admin: gurney
team: server-foundation
server-foundation team admin: le, jian
server-foundation team view: dang


## Gurney:
1. create a managedclusterset, bind the admin permission to server-foundation-admin, bind the view permission to server-foundation-view
2. create a project(server-foundation) for server-foundation team. bind the admin permission to server-foundation-admin.

Notes: cluster-admin can also give the self-provisioner permissions to server-foundation team-admin, then the team-admin can create it by themselves.

## Le:
 1. create a managedclusters c1
 2. create a clusterpool
 3. create a clusterclaim

## jian:
1. can get/update/delete managedclusters
2. can  get/update/delete clusterpool
3. can get/update/delete  clusterclaim

### dang:
1. can view ...
