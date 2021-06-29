# Helm Charts

## Install - httpbin chart devel.
> tgz파일 직접 지정하는 방법


```
# tgz파일 직접 Install
▒ helm install release-devel https://raw.githubusercontent.com/itnpeople/k8s.docs/master/charts/httpbin-devel.tgz

# context apps-05 에 배포
▒ helm install release-devel https://raw.githubusercontent.com/itnpeople/k8s.docs/master/charts/httpbin-devel.tgz --kube-context apps-05

# 배포 확인
▒ kubectl get pod -n default --context apps-05| grep httpbin

httpbin-9f6dd8548-9jt9x           1/1     Running   0          59s
```


### Install - acornsoft-dashboard
> repository 추가를 통한 방법

```
# "itnp-kr" repository 추가
▒ helm repo add itnp-kr https://raw.githubusercontent.com/itnpeople/k8s.docs/master/charts
▒ helm repo list

# "itnp-kr" repository 에서 설치 가능 차트 조회
▒ helm search repo itnp-kr

# Test
▒ helm install -n acornsoft-dashboard acornsoft-dashboard itnp-kr/acornsoft-dashboard \
  --dry-run --debug \
  --version 0.1.2 \
  --set dashboard.service.type=NodePort \
  --set dashboard.service.nodePort=30090 \
  --set backend.service.type=NodePort \
  --set backend.service.nodePort=30081 \
  --set frontend.service.type=NodePort \
  --set frontend.service.nodePort=30080

# install
▒ kubectl create ns acornsoft-dashboard
▒ helm install -n acornsoft-dashboard acornsoft-dashboard itnp-kr/acornsoft-dashboard \
  --version 0.1.2 \
  --set dashboard.service.type=NodePort \
  --set dashboard.service.nodePort=30090 \
  --set backend.service.type=NodePort \
  --set backend.service.nodePort=30081 \
  --set frontend.service.type=NodePort \
  --set frontend.service.nodePort=30080
```

* Repoisitory URL 에 index.yaml 존재해야 함.
* `index.yaml` 파일 생성/수정 방법은 `helm repo index` 명령 사용
```
▒ helm repo index .
```


## development

### 배포 테스트

* helm template 명령으로 배포 테스트

```
▒ helm template httpbin . --namespace default | kubectl apply -f -
```

*  포트 포워딩을 사용하여 접속 확인 (http://localhost:8080 , loadBalancer가 없는 on-premise)

```
▒ kubectl port-forward $(kubectl get pod -l app=httpbin -o jsonpath={.items..metadata.name}) 8080:80
```

* external ip 확인 (public cloud ) 

```
▒ echo http://$(kubectl get svc/httpbin -o jsonpath={.status.loadBalancer.ingress[0].ip})
```

### helm 3 패키징

* 패키징 (httpbin-devel.tgz) 파일 생성

```
▒ helm package .
```

```
# install
▒ helm install release-devel httpbin-devel.tgz

# install 확인
▒ helm list
NAME         	NAMESPACE    	REVISION    UPDATED                                 STATUS  	CHART           APP VERSION
release-devel	default	        1           2020-05-20 15:37:50.589253 +0900 KST    deployed    httpbin-devel   1.0.0
```

* 패키징 파일 github으로 push
```
▒ git add *
▒ git commit -m 'ADD httpbin helm chart'
▒ git push origin master
```

