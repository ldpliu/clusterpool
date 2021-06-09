# I will show the RBAC around managedClusterset.
I will have a demo for that. 
In the demo, we define some persona, gurney, he is cluster-admin, 
and we have a team, server-foundation, in this team, we have three members, le,jian,dangpeng,
le and jian are team-admin, dangpeng is team-view. we will use these persona to login to ocp and acm-ui to explain the rbac around mangedclusters.



1. let's login to OCM ui as gurney,
and , let's create a managedclusterset named server-foundation-clusterset, it's used for server-foundation team

then, 
2. let's switch to ocp ui as gurney.
3. we have define two group server-foundation-admin server-foundation-view

in server-foundation-admin, there are two users, le-taem-admin, jian-team-admin
in server-foundation-view there is one user, dangpeng-team-view


then, let's create some clusterrolr-binding to grant managedclusterset permission to server-foundation team.
1. for each managedclusterset, we will auto generate two clusterrole, one for managedclusterset admin, another is manageclusterset view.

1. we need to give the server-foundation-admin group server-foundation-clusterset admin permission
2. let's give server-foundation-vewi group server-foundation-clusterset view permission

for each team-admin, they should provision cluster by them selves. so we should give the self-provisioner permission to server-foubdaiton-admin group.



# let's login to openshift ui as le-team-admin, then create a project named "server-foundation"

then login to ocm ui as le-team-admin and provision some clusters.

1. let's create a managedcluster managedcluster1, as non-cluster-admin, he must select managedclusterset, or the create request will fail.
2. let's create a clusterpool named "server-foundation-clusterpool", same as mangedclusters, he must select managedclusterset.
3.  then he can claim the clusterpool.
4.  in clusterset, we can see these resources.




# login to ocm ui as jian-team-admin
1. he also has admin permissions to the resources which created by le-team-admin.
2. let's try to add one label for mangaedcluster1
3. let's try to add a lable for clusterpool.


# login to ocm ui as dangpeng-team-view
1. he has view permissions to the resources which created by le-team-admin.
2. he can see these resources, but he can not update it. 
