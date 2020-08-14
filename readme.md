## Grafana
1. Deploy EKS service with the load balancer:
```bash
helm install grafana-sample  stable/grafana --set service.type=LoadBalancer
```
1. Run the following to get the ALB address:
```bash
export SERVICE_ADDR=$(kubectl get svc --namespace default grafana-sample -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
echo http://$SERVICE_ADDR:80
```
1. Read the grafana admin password by running the following:
```bash
kubectl get secret --namespace default grafana-sample -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
kubectl expose deployment grafana-sample  --type=LoadBalancer  --name=grafana-service

## Prometheus
1. Install the stable helm chart:
```bash
helm install prometheus-sample  stable/prometheus --set server.service.type=LoadBalancer
```
1. The output will contain the number of commands to find the desired access points but since we specified the `LoadBalancer` as service type run the following commands to get the public access point:
```bash
export SERVICE_ADDR=$(kubectl get svc --namespace default prometheus-sample-server -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
echo http://$SERVICE_ADDR:80
```
