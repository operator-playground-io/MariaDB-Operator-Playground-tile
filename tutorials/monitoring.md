---
title: Enable Monitoring to MariaDB
description: This tutorial explains how to Enable Monitoring service to MariaDB
---

### Enable Monitoring service on MariaDB 

Step 1: 

- Execute below command to get services of MariadB in "my-mariadb-operator-app" namespace:
  
  ```execute
  kubectl get svc -n my-mariadb-operator-app
  ```

 Output:
 ```
      NAME                       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
 mariadb-operator-metrics   ClusterIP   10.111.158.91    <none>        8383/TCP,8686/TCP   3d4h
 mariadb-service            NodePort    10.106.178.202   <none>        80:30685/TCP        3d4h
 ```
 
- Get the port of mariadb-service:
  
  From above command's output, mariadb-service port is 30685 

- To enable monitoring using Prometheus exporter pod and service, create the below yaml definition of the Custom Resource:


```execute
cat <<'EOF'> MariaDBmonitoring.yaml
apiVersion: mariadb.persistentsys/v1alpha1
kind: Monitor
metadata:
  name: mariadb-monitor
spec:
  # Add fields here
  size: 1
  # Database source to connect with for colleting metrics
  # Format: "<db-user>:<db-password>@(<dbhost>:<dbport>)/<dbname>">
  # Make approprite changes 
  dataSourceName: "root:password@(##DNS.ip##:30685)/test-db"
  # Image name with version
  # Refer https://registry.hub.docker.com/r/prom/mysqld-exporter for more details
  image: "prom/mysqld-exporter"
EOF
```

Note: The database host and port should be correct for metrics to work.



Step 2: Execute below command to Create Instance of Monitoring: 

```execute
kubectl create -f MariaDBmonitoring.yaml -n my-mariadb-operator-app
```

Output:


```
monitor.mariadb.persistentsys/mariadb-monitor created
```

This will start Prometheus exporter pod and service. 




## Create monitoring resources 

To enable monitoring services, you need to :

1. Install Prometheus Operator and Deploy Prometheus Instance and ServiceMonitor 

2. Install Grafana Operator and Deploy Grafana Server Instance and Grafana Data-Source

If above prerequisites are already fulfilled, you can proceed with Step 2 and Step 4.

Else proceed as follows: 


### Step 1: Install Prometheus operator and Deploy Prometheus Instance and ServiceMonitor
 
 Learn how to Install Prometheus operator and Deploy Prometheus Instance and ServiceMonitor from tutorial "6: Create Prometheus Operator".


### Step 2: Verify prometheus monitoring metrics

Access the Prometheus dashboard using below URL:

http://##DNS.ip##:30100

- On the prometheus UI, Go to Status -> Targets to see endpoints.


 ![](_images/targets.PNG)

Note: It may take some time to visible Target's endpoints.

- From the dropdown you can select the query and click on "Execute" to see MariaDB Metrics. See below snapshot :


![](_images/queryexecution.PNG)




### Step 3: Install Grafana Operator and Deploy Grafana Server and Gafana Data-Source


Learn how to Install Grafana Operator and Deploy Grafana Server and Gafana Data-Source from tutorial "7: Create Grafana Operator".



### Step 4: Access Grafana dashboard


- Execute below command to get all services in "my-grafana-operator" namespace:


```execute
kubectl get svc -n my-grafana-operator
```


Output :

```
NAME                       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
grafana                    NodePort    10.101.18.57    <none>        9090:30200/TCP   5m31s
grafana-operator-metrics   ClusterIP   10.99.122.95    <none>        8080/TCP         6m45s
grafana-service            ClusterIP   10.109.139.18   <none>        3000/TCP         5m53s
```

Port value of "grafana" Service of TYPE NodePort is : 30200


We can access the Grafana dashboard on the nodePort : 30200 using below url:

<a href="http://##DNS.ip##:30200" target="_blank">http://##DNS.ip##:30200</a> 


You will see the Grafana page loading as below :


![](_images/load.png)


Now click on the `Sign In` button as below :

![](_images/signin.png)

Log-in to Grafana Dashboard with the following credentials in the page below:


```
user: root
password: secret
```
![](_images/login.png)


Now you will be able to see the Dashboard like below:


![](_images/dashboard.png)


### Configure Grafana Dashboard to visualize MariaDB monitoring metrics

Learn how to Configure Grafana Dashboard to visualize MariaDB monitoring metrics from this tutorial "8: Grafana Dashboard".





