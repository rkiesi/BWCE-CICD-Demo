# Kubernetes Web-UI Configuration

A full explanation on how to install and access Kubernetes-UI on Rancher/K3s is profided [here](https://rancher.com/docs/k3s/latest/en/installation/kube-dashboard/).

First the K8s-UI must be deployed. The appropriate YAML is picked from a github repo:
```
GITHUB_URL=https://github.com/kubernetes/dashboard/releases
VERSION_KUBE_DASHBOARD=$(curl -w '%{url_effective}' -I -L -s -S ${GITHUB_URL}/latest -o /dev/null | sed -e 's|.*/||')
sudo k3s kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/${VERSION_KUBE_DASHBOARD}/aio/deploy/recommended.yaml
```

Next we need to deploy the role and user suff:
`sudo k3s kubectl create -f dashboard.admin-user.yml -f dashboard.admin-user-role.yml`

Now Obtain the Bearer Token
`sudo k3s kubectl -n kubernetes-dashboard describe secret admin-user-token | grep '^token'`

And anble local access
`sudo k3s kubectl proxy`

The Dashboard is now accessible at:

[http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/)
Sign In with the admin-user Bearer Token


