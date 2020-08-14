## Prometheus
### Description
There is no need to do extra effort to install prometheus. You can grab the existing helm chart and install it with a small customisation.

### Installation steps
1. Install the stable helm chart:
```bash
helm install prometheus-sample  stable/prometheus --set server.service.type=LoadBalancer
```
2. The output will contain the number of commands to find the desired access points but since we specified the `LoadBalancer` as service type run the following commands to get the public access point:
```bash
export PROMETHEUS_SERVICE_ADDR=$(kubectl get svc --namespace default prometheus-sample-server -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
echo http://$PROMETHEUS_SERVICE_ADDR:80
```

## Grafana
### Description
There is no need to do extra effort to install `Grafana`. You can grab the existing helm chart and install it with a small customisation.
This example includes only one dashboard that prints the `mysql_slow_query` results. The slow logs metric is being exported to prometheus server using the `mysql_node_exporter` in [mysql chart](https://github.com/yyarmoshyk/mysql-kubernetes-with-prometheus-exporter)

### Installation steps
1. Deploy EKS service with the load balancer:
```bash
helm install grafana-sample  stable/grafana --set service.type=LoadBalancer
```
2. Run the following to get the ALB address:
```bash
export GRAFANA_SERVICE_ADDR=$(kubectl get svc --namespace default grafana-sample -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
echo http://$GRAFANA_SERVICE_ADDR:80
```
3. Read the grafana admin password by running the following:
```bash
kubectl get secret --namespace default grafana-sample -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
4. Login to the Grafana web interface using the `$SERVICE_ADDR` and the admin password.
5. Add prometheus datasource using the `$PROMETHEUS_SERVICE_ADDR`
6. Import the [dashboard](https://github.com/yyarmoshyk/grafana-and-prometheus-kubernetes/blob/master/grafana-dashboards/mysql_slow_query_log.json).

Really great set of MySQL and Aurora dashboards is available in [percona project](https://github.com/percona/grafana-dashboards)
