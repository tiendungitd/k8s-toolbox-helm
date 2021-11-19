### NGINX ingress controller ###
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
kubectl create ns ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx -f nginx-custom.yaml -n ingress-nginx..

```
### EFS Provisioner ###
``` bash
helm install efs-provisioner stable/efs-provisioner -n efs -f efs-provisioner-value.yaml
```
### EFK ###
Execute the following command to add the Bitnami charts repository to Helm:
``` bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```
Elasticsearch
``` bash
kubectl create ns efk
helm install elasticsearch bitnami/elasticsearch -f es_value.yaml -n efk
```
Kibana
``` bash
# Create logtrail.json
kubectl create -f logtrail-cm.yaml
# create tls sercet for ingress
kubectl create -f aifs-tls-secret.yaml
# create basic auth for ingress
kubectl create -f basic-auth-secret.yaml
helm install kibana bitnami/kibana -n efk -f kibana-value.yaml
```
Fluentd
``` bash
helm repo add kokuwa https://kokuwaio.github.io/helm-charts
kubectl create -f fluentd-config.yaml
helm install fluentd-elasticsearch kokuwa/fluentd-elasticsearch -f kokuwaio-fluentd-value.yaml -n efk
```
### Reference ###
* https://github.com/kokuwaio/helm-charts/tree/main/charts/fluentd-elasticsearch
* https://github.com/kubernetes/ingress-nginx/tree/master/charts/ingress-nginx
* https://docs.bitnami.com/tutorials/integrate-logging-kubernetes-kibana-elasticsearch-fluentd/
