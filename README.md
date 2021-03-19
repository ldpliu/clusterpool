# Server-foundation Clusterpool Guide
1. 用github 账号登录到
https://console-openshift-console.apps.collective.aws.red-chesterfield.com/ 
拷贝登录命令
2. 粘贴登录命令到本地计算机
3. 创建clusterpool
```apiVersion: hive.openshift.io/v1
kind: ClusterPool
metadata:
 name: server-foundation-aws
 namespace: server-foundation
spec:
 baseDomain: dev04.red-chesterfield.com
 imageSetRef:
   name: img4.6.16-x86-64-appsub
 size: 2
 pullSecretRef:
   name: server-foundation-ocp-pull-secret
 platform:
   aws:
     credentialsSecretRef:
       name: server-foundation-aws-creds
     region: us-east-1
```
4. 创建clusterclaim
```
apiVersion: hive.openshift.io/v1
kind: ClusterClaim
metadata:
 name: server-foundation-aws-ldp3
 namespace: server-foundation
spec:
 clusterPoolName: server-foundation-aws
 subjects:
 - apiGroup: rbac.authorization.k8s.io
   kind: Group
   name: Server Foundation
```
5. 从clusterclaim 的spec.namespace 拿到clusterdeployment 的name和namespace
```
kubectl get clusterclaim.hive.openshift.io -n server-foundation -oyaml
```
6. 在clusterdeoloyment里面有kubeconfig和ui url， password等
```
kubectl get clusterdeployment server-foundation-aws-tfzc8 -n server-foundation-aws-tfzc8 -oyaml
```

## 当前环境的状态
一个size=2的clusterpool，一个cliam

```[root@dangpeng deploy]# kubectl get clusterpool -n server-foundation
NAME                    READY   SIZE   BASEDOMAIN                   IMAGESET
server-foundation-aws   1       2      dev04.red-chesterfield.com   img4.6.16-x86-64-appsub


[root@dangpeng deploy]# kubectl get  clusterclaim.hive.openshift.io -n server-foundation
NAME                         AGE
server-foundation-aws-ldp4   52m
```

