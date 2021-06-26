# Hello World Sample App using kubernetes by hitlur

Nothing much

## How to run

```
kubect apply -f create-deployment.yaml
```
```kubectl get deployments
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
my-app-deployment1   1/1     1            1           9m50s
```

```kubectl get pods
NAME                                  READY   STATUS    RESTARTS   AGE
my-app-deployment1-68757c49b5-cvr8c   1/1     Running   0          10m
```


if the application is already running, to make sure we will enter the pod
```
kubectl exec -it my-app-deployment1-68757c49b5-cvr8c -- /bin/sh
```
```
/app # netstat -tulpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 :::8080                 :::*                    LISTEN      1/binary
```
```
/app # apk add curl
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/community/x86_64/APKINDEX.tar.gz
(1/1) Installing curl (7.67.0-r4)
Executing busybox-1.31.1-r9.trigger
OK: 22 MiB in 21 packages
```
```
/app # curl localhost:8080
hello world/app #
```
```
/app # curl -I -X GET localhost:8080
HTTP/1.1 200 OK
Date: Tue, 22 Jun 2021 02:05:44 GMT
Content-Length: 11
Content-Type: text/plain; charset=utf-8
```

### mysql, volume and secret
```
kubectl create configmap structure-sql --from-file={{ path }}/schema.sql

kubectl get configMap
NAME               DATA   AGE
kube-root-ca.crt   1      21h
structure-sql      1      17s

kubectl describe configmaps structure-sql
Name:         structure-sql
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
schema.sql:
----
CREATE DATABASE IF NOT EXISTS hello_world;
USE hello_world;

CREATE TABLE IF NOT EXISTS users (
    id int NOT NULL AUTO_INCREMENT,
    first_name VARCHAR(255) NOT NULL,
    last_name VARCHAR(255),
    birth DATETIME,
    PRIMARY KEY (id)
);
Events:  <none>

```

Testing from pod app use mysql client for connect to database
```
 kubectl get pod
NAME                                  READY   STATUS    RESTARTS   AGE
my-app-deployment1-57cd6b9b4b-nbmkd   1/1     Running   1          3h19m
my-app-deployment2-8bd54f888-6tjdg    1/1     Running   0          6m1s

kubectl get service
NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
my-service1      NodePort    10.98.120.1    <none>        8080:30318/TCP   3h7m
my-service2      ClusterIP   10.97.94.38    <none>        3306/TCP         3h9m

kubectl exec -it my-app-deployment1-57cd6b9b4b-nbmkd -- /bin/sh
/app # mysql -uroot -P3306 -h 10.97.94.38 -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 7
Server version: 10.5.11-MariaDB-1:10.5.11+maria~focal mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| hello_world        |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)

MariaDB [(none)]> use hello_world;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [hello_world]> show tables;
+-----------------------+
| Tables_in_hello_world |
+-----------------------+
| users                 |
+-----------------------+
1 row in set (0.001 sec)
```
## end
