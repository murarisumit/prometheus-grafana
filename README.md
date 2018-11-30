### Create a namespace for monitoring:

* `kubectl create ns monitoring`

### Installing prometheus

All these are instruction are in `Makefile` too, but full instruction are there below too.

```
make install
make port-foward
```

* `helm install --name prometheus-monitoring stable/prometheus --namespace monitoring`

* Below are some of details spit out after installation
```
    The Prometheus server can be accessed via port 80 on the following DNS name from within your cluster:
    prometheus-monitoring-server.monitoring.svc.cluster.local


    Get the Prometheus server URL by running these commands in the same shell:
      export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")
      kubectl --namespace monitoring port-forward $POD_NAME 9090


    The Prometheus alertmanager can be accessed via port 80 on the following DNS name from within your cluster:
    prometheus-monitoring-alertmanager.monitoring.svc.cluster.local


    Get the Alertmanager URL by running these commands in the same shell:
      export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=prometheus,component=alertmanager" -o jsonpath="{.items[0].metadata.name}")
      kubectl --namespace monitoring port-forward $POD_NAME 9093


    The Prometheus PushGateway can be accessed via port 9091 on the following DNS name from within your cluster:
    prometheus-monitoring-pushgateway.monitoring.svc.cluster.local


    Get the PushGateway URL by running these commands in the same shell:
      export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=prometheus,component=pushgateway" -o jsonpath="{.items[0].metadata.name}")
      kubectl --namespace monitoring port-forward $POD_NAME 9091
```
### Set port-forwarding to see if prometheus is working fine.

* `export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")`
* `kubectl --namespace monitoring port-forward $POD_NAME 9090`

For alertmanager and others see the helpful notes docs that shows on command line after helm install.

### Installing grafana

* Create a file called values.yaml with content from here: [https://gist.github.com/murarisumit/172465c2dbc3383defaedca70f8b2707](https://gist.github.com/murarisumit/172465c2dbc3383defaedca70f8b2707)

These instruction are there in `Makefile` too but full description below
```
make install
make ls
make port-foward
```


* `helm install --name grafana stable/grafana --version 1.11.6 --namespace monitoring -f values.yaml`

* Below are some of details spit out after installation

```
1. Get your 'admin' user password by running:

   kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

2. The Grafana server can be accessed via port 80 on the following DNS name from within your cluster:

   grafana.monitoring.svc.cluster.local

   Get the Grafana URL to visit by running these commands in the same shell:

     export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=grafana,component=" -o jsonpath="{.items[0].metadata.name}")
     kubectl --namespace monitoring port-forward $POD_NAME 3000

3. Login with the password from step 1 and the username: admin

```

`export POD_NAME=.....` didn't worked for me instead tried it

```
export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=grafana" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace monitoring port-forward $POD_NAME 3000
```

Now goto `http://locahost:3000`, enable kuberenetes plugin, then put your credentials about how you want grafana to talk to kuberentes. 


This works for getting started with prometheus and grafana.


Reference: 
* [https://rohanc.me/monitoring-kubernetes-prometheus-grafana/](https://rohanc.me/monitoring-kubernetes-prometheus-grafana/)
