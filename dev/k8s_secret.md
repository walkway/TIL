## Kubernetes > Secret
```
// secret.yaml 
apiVersion: v1
kind: Secret
metadata:
  name: loglevel-secret
data:
  loglevel: U0UtSU5GTw==
```
```
// deployment.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: k8s-configmap-deployment
  labels:
    app: k8s-configmap
spec:
  replicas: 1
  selector:
    matchLabels:
      app: k8s-configmap
  template:
    metadata:
      labels:
        app: k8s-configmap
    spec:
      containers:
      - name: k8s-configmap
        image: local/springboot:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
        env:
        - name: SECRET_LOG_LEVEL
          valueFrom:
            secretKeyRef: 
              name: loglevel-secret
              key: loglevel
```
```
// application.properties
secret.log.level=${SECRET_LOG_LEVEL}
```
```
@Value("${echo.secret.log.level}")
String echoSecretLogLevel;
```
