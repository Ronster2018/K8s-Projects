# Steps to set up the K8s provided dashboard using Token Based Authentication

1. Run the following command
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

2. Next, we need to create a sample user. Copy the following snippet to a `dashboard-adminuser.yaml` file.
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
```

3. Next, copy the following into a rolebinding.yaml file
``` yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

4. Apply the yaml files.
```bash
kubectl apply -f dashboard-adminuser.yaml
kubectl apply -f rolebinding.yaml
```

5. Get the Bearer Token
```bash
kubectl -n kubernetes-dashboard create token admin-user
```

6. To clean up from the dashboard
``` bash
kubectl -n kubernetes-dashboard delete serviceaccount admin-user
kubectl -n kubernetes-dashboard delete clusterrolebinding admin-user
```