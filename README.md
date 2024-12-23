# Grafana-Corda

## Installation and configuration

# Grafana UI + Prometheus

Create namespace

~~~

kubectl create namespace obsevability

~~~

Upload the files from the repository to cloud shell or vm

~~~

cd engineering-helm-main-updated\charts\observability
helm dependency build

~~~

~~~

cd ..
helm install diag1 observability -n observability

~~~

Go to the folder where corda-pod-monitor.yaml is located and run

~~~

kubectl apply -n observability -f corda-pod-monitor.yaml

~~~

Get the admin password

~~~

kubectl get secret --namespace observability diag1-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

~~~

Check pods and note grafana pod name in order to expose it

~~~

kubectl get pod -n observability

kubectl expose pod <diag1-grafana-...> --type=LoadBalancer --name=grafana-service --port=3000 --target-port=3000 -n observability
~~~

Get your public ip

~~~

kubectl get svc -n observabiltiy

~~~

In your browser

~~~

<service public ip>:3000

username: admin
password: output of the previous command 
~~~

# Loki 

## Loki without grafana ui

~~~

kubectl create namespace logging

~~~


Go to the folder where lokionly.yaml is available

~~~

helm install loki-logging grafana/loki-stack -n logging -f lokionly.yaml

~~~

Go to your grafana webpage and follow these steps to add loki

Go to menu ---> connections ---> add new conenction 
Choose Loki


![](./Images/Screenshot%202024-06-24%20115751.png)

url=
~~~
http://loki-logging.logging.svc.cluster.local:3100
~~~

Save And Exit

If you get an error, wait a couple minutes in order for loki to be fully deployed on the cluster


## Loki + Grafana UI

Follow this part if you only need loki without prometheus

~~~

kubectl create namespace logging

~~~


Go to the folder where loki+grafana.yaml is available

~~~

helm install loki-logging grafana/loki-stack -n logging -f loki+grafana.yaml

~~~



Get the admin password

~~~

kubectl get secret --namespace logging loki-logging-grafana -ojsonpath="{.data.admin-password}" | base64 --decode ; echo

~~~

Check pods and note grafana pod name in order to expose it

~~~

kubectl get pod -n logging

kubectl expose pod <loki-logging-grafana-...> --type=LoadBalancer --name=grafana-service --port=3000 --target-port=3000 -n logging
~~~

Get your public ip

~~~

kubectl get svc -n logging

~~~

In your browser

~~~

<service public ip>:3000

username: admin
password: output of the previous command 
~~~






# Postgresql

Follow these steps to add postgresql to grafana

Go to menu ---> connections ---> add new conenction 
Choose Postgresql

Postgres service should be exposed in order to connect

![](./Images/Screenshot%202024-06-24%20120124.png)

Save and Exit






















