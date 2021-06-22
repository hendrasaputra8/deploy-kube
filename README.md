# Hello World Sample App using kubernetes 

Nothing much

## How to run

kubect apply -f create-deployment.yaml

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
## end
