# Demo

## BookInfo

* Deploy

```
$ kubectl create ns bookinfo 
$ kubectl apply -n bookinfo -f https://raw.githubusercontent.com/istio/istio/master/samples/bookinfo/platform/kube/bookinfo.yaml
```

## Sock Shop
> https://microservices-demo.github.io/ 
> https://github.com/microservices-demo/microservices-demo

* Deploy
  * 원본 : https://raw.githubusercontent.com/microservices-demo/microservices-demo/master/deploy/kubernetes/complete-demo.yaml

```
$ kubectl apply -f https://raw.githubusercontent.com/itnpeople/k8s.docs/master/demo/yaml/sock-shop.yaml
```

* Open `http://<cluster ip>:30001/` in your browser

## Weave Scope
> https://github.com/microservices-demo/microservices-demo
> https://github.com/weaveworks/scope
> https://microservices-demo.github.io/

```
$ kubectl apply -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '\n')"
$ kubectl port-forward -n weave service/weave-scope-app 4040:80
```

## Kubernetes example applications
> https://github.com/kubernetes/examples

* Guestbook

```
$ kubectl create ns guestbook
$ kubectl apply -n guestbook -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook/all-in-one/guestbook-all-in-one.yaml
```

## locust
> https://locust.io/


* Deploy the target application "podinfo"
```
$ kubectl apply -f - <<EOF
apiVersion: apps/v1 
kind: Deployment 
metadata: 
  name: podinfo 
spec: 
  selector: 
    matchLabels: 
      app: podinfo 
  template: 
    metadata: 
      labels: 
        app: podinfo 
    spec: 
      containers: 
      - name: podinfo 
        image: stefanprodan/podinfo 
        ports: 
        - containerPort: 9898 
--- 
apiVersion: v1 
kind: Service 
metadata: 
  name: podinfo 
spec: 
  ports: 
    - port: 80 
      targetPort: 9898 
  selector: 
    app: podinfo 
EOF
```


* Deploy "locust"

```
$ kubectl apply -f - <<EOF
apiVersion: v1 
kind: ConfigMap 
metadata: 
  name: locust-script 
data: 
  locustfile.py: |- 
    from locust import HttpUser, task, between 

    class QuickstartUser(HttpUser): 
        wait_time = between(0.7, 1.3) 

        @task 
        def hello_world(self): 
            self.client.get("/", headers={"Host": "example.com"})
--- 
apiVersion: apps/v1 
kind: Deployment 
metadata: 
  name: locust 
spec: 
  selector: 
    matchLabels: 
      app: locust 
  template: 
    metadata: 
      labels: 
        app: locust 
    spec: 
      containers: 
        - name: locust 
          image: locustio/locust 
          ports: 
            - containerPort: 8089 
          volumeMounts: 
            - mountPath: /home/locust 
              name: locust-script 
      volumes: 
        - name: locust-script 
          configMap: 
            name: locust-script 
EOF
```

* Open in your browser `http://localhost:8089` 
* Enter `http://podinfo` in the **Host** parameter value.

```
$ kubectl port-forward deploy/locust 8089:8089
```

