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
