1. clusteradmin create clusterset `server-foundation-clusterset`

```yaml
oc apply -f managedclusterset.yaml
```

2. clusteradmin grant permission to server-foundation team
    2.1 clusteradmin grant `self-provisioner` permission to `server-foundation-team-admin`

    2.2 clusteradmin grant `clusterset admin` permission to `server-foundation-team-admin`

    2.3 clusteradmin grant `clusterset view` permission to `server-foundation-team-view`
```yaml
oc adm policy add-cluster-role-to-user open-cluster-management:clusterset-admin:server-foundation-clusterset le jian

oc adm policy add-cluster-role-to-user open-cluster-management:clusterset-view:server-foundation-clusterset dangpeng

oc adm policy add-cluster-role-to-group self-provisioner le jian
```
3. As team-admin le create a project `managedcluster1`

```yaml
oc new-project managedcluster1 --as le
```
4. As team-admin, Le creates a managedcluster managedcluster1 with clusterset label 
```yaml
oc apply -f managedcluster1.yaml
```
5. As another team-admin, jian can edit managedcluster1.
```yaml
oc get managedclusters
oc label managedcluster managedcluster1 testlabel=test
```
6. as team-user, dangpeng cann view managedcluster1

```yaml
oc get managedclusters
oc label managedcluster managedcluster1 testlabel=test1 --as jian
```



