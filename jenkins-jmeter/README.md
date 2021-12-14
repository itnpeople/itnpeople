# Jenkins-JMeter
> JMeter 활용한 위한 Jenkins 이미지


## 빌드

~~~
▒ docker build . --tag honester/jenkins-jmeter:latest
▒ docker push honester/jenkins-jmeter:latest
~~~

## 실행

### Docker

~~~
▒ docker run --rm -d -p 8080:8080 \
  --name jenkins \
  -e JENKINS_USERNAME="admin" \
  -e JENKINS_PASSWORD="admin1234" \
  honester/jenkins-jmeter:latest

▒ docker logs jenkins -f

....
2021-12-14 04:10:32.281+0000 [id=56]	INFO	hudson.model.AsyncPeriodicWork#lambda$doRun$0: Finished Download metadata. 4,394 ms
~~~

* open in your browser http://localhost:8080/
* enter userid & password : admin/admin1234

### Kubernetes

~~~
▒ kubectl apply -f - <<EOF
kind: Deployment
apiVersion: apps/v1
metadata:
  name: jenkins-jmeter
  labels:
    app.kubernetes.io/name: jenkins-jmeter
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/name: jenkins-jmeter
  template:
    metadata:
      labels:
        app.kubernetes.io/name: jenkins-jmeter
    spec:
      containers:
        - name: jenkins
          image: honester/jenkins-jmeter:latest
          imagePullPolicy: IfNotPresent
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
            - name: https
              containerPort: 8443
              protocol: TCP
          env:
            - name: JENKINS_USERNAME
              value: admin
            - name: JENKINS_PASSWORD
              value: admin1234
          volumeMounts:
            - name: jenkins-data-volume
              mountPath: /bitnami/jenkins
      volumes:
        - name: jenkins-data-volume
          emptyDir: {}
      nodeSelector:
        "kubernetes.io/os": linux
---
kind: Service
apiVersion: v1
metadata:
  name: jenkins-jmeter
  labels:
    app.kubernetes.io/name: jenkins-jmeter
spec:
  type: NodePort
  ports:
    - name: jenkins
      nodePort: 30084
      port: 8080
      protocol: TCP
      targetPort: 8080
  selector:
    app.kubernetes.io/name: jenkins-jmeter
EOF
~~~

* open in your browser http://<cluster-ip>:30084/
* enter userid & password : admin/admin1234


## 참고

* https://jmeter.apache.org/
* https://hub.docker.com/r/bitnami/jenkins/
* https://github.com/bitnami/bitnami-docker-jenkins
