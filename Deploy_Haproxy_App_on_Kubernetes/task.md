The Nautilus project team needs to setup a HaProxy app on Kubernetes cluster. In the recent meeting they discussed all requirements and how implementation details will proceed. Below you can find more details and proceed accordingly:
1. Create a namespace haproxy-controller-xfusion.
```
apiVersion: v1
kind: Namespace
metadata: 
  name: haproxy-controller-xfusion
```
2. Create a ServiceAccount haproxy-service-account-xfusion under the same namespace.
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: haproxy-service-account-xfusion
  namespace: haproxy-controller-xfusion
  ```
3. Create a ClusterRole which should be named as haproxy-cluster-role-xfusion, rules are placed under location /tmp/rules.yml on jump_host.    Use the content of file /tmp/rules.yml to configure ClusterRole.
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: haproxy-cluster-role-xfusion
  namespace: haproxy-controller-xfusion
rules:
- apiGroups: [""]
  resources: ["configmaps", "endpoints", "nodes", "pods", "services", "namespaces", "events", "serviceaccounts"]
  verbs: ["get", "list", "watch"]

- apiGroups: ["extensions"]
  resources: ["ingresses", "ingresses/status"]
  verbs: ["get", "list", "watch", "update"]
  
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "list", "watch", "create", "patch"]
```
4. Create a ClusterRoleBinding which should be named as haproxy-cluster-role-binding-xfusion under the same namespace. Define roleRef apiGroup should be rbac.authorization.k8s.io, kind should be ClusterRole, name should be haproxy-cluster-role-xfusion and subjects kind should be ServiceAccount, name should be haproxy-service-account-xfusion and namespace should be haproxy-controller-xfusion.
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: haproxy-cluster-role-binding-xfusion
  namespace: haproxy-controller-xfusion
subjects:
- kind: ServiceAccount
  name: haproxy-service-account-xfusion
  namespace: haproxy-controller-xfusion
roleRef:
  kind: ClusterRole
  name: haproxy-cluster-role-xfusion
  apiGroup: rbac.authorization.k8s.io
  ```
5. Create a backend deployment which should be named as backend-deployment-xfusion under the same namespace, labels run should be ingress-default-backend under metadata. Configure spec as replica should be 1, selector's matchLabels run should be ingress-default-backend. Template's labels run under metadata should be ingress-default-backend. The container should named as backend-container-xfusion, use image gcr.io/google_containers/defaultbackend:1.0 ( use exact name of image as mentioned ) and its containerPort should be 8080.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-deployment-xfusion
  namespace: haproxy-controller-xfusion
  labels:
    run: ingress-default-backend
spec:
  replicas: 1
  selector:
    matchLabels:
      run: ingress-default-backend
  template:
    metadata:
      labels:
        run: ingress-default-backend
    spec:
      containers:
      - name: backend-container-xfusion
        image: gcr.io/google_containers/defaultbackend:1.0
        ports:
        - containerPort: 8080
```
6. Create a service for backend which should be named as service-backend-xfusion under the same namespace, labels run should be ingress-default-backend. Configure spec as selector's run should be ingress-default-backend, port should be named as port-backend, protocol should be TCP, port should be 8080 and targetPort should be 8080.
```
apiVersion: v1
kind: Service
metadata:
  name: service-backend-xfusion
  namespace: haproxy-controller-xfusion
  labels:
    run: ingress-default-backend
spec:
  selector:
    run: ingress-default-backend
  ports:
    - name: port-backend
      protocol: TCP
      port: 8080
      targetPort: 8080
```
7. Create a deployment for frontend which should be named haproxy-ingress-xfusion under the same namespace. Configure spec as replica should be 1, selector's matchLabels should be haproxy-ingress, template's labels run should be haproxy-ingress under metadata. The container name should be ingress-container-xfusion under the same service account haproxy-service-account-xfusion, use image haproxytech/kubernetes-ingress, give args as --default-backend-service=haproxy-controller-xfusion/service-backend-xfusion, resources requests for cpu should be 500m and for memory should be 50Mi, livenessProbe httpGet path should be /healthz its port should be 1024. The first port name should be http and its containerPort should be 80, second port name should be https and its containerPort should be 443 and third port name should be stat its containerPort should be 1024. Define environment as first env name should be TZ its value should be Etc/UTC, second env name should be POD_NAME its valueFrom fieldRef fieldPath should be metadata.name and third env name should be POD_NAMESPACE its valueFrom fieldRef fieldPath should be metadata.namespace.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: haproxy-ingress-xfusion
  namespace: haproxy-controller-xfusion
  labels:
    run: haproxy-ingress
spec:
  replicas: 1
  selector:
    matchLabels:
      run: haproxy-ingress
  template:
    metadata:
      labels:
        run: haproxy-ingress
    spec:
      serviceAccountName: haproxy-service-account-xfusion
      containers:
      - name: ingress-container-xfusion
        image: haproxytech/kubernetes-ingress
        resources:
          requests:
            cpu: 500m
            memory: 50Mi
        args: 
        - --default-backend-service=haproxy-controller-xfusion/service-backend-xfusion
        livenessProbe:
          httpGet:
            path: /healthz
            port: 1024
        ports:
        - name: http
          containerPort: 80
        - name: https
          containerPort: 443
        - name: stat
          containerPort: 1024
        env:
        - name: TZ
          value: Etc/UTC
        - name: POD_NAME
          valueFrom:
            fieldRef: 
              fieldPath: metadata.name
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef: 
              fieldPath: metadata.namespace
      
```
8. Create a service for frontend which should be named as ingress-service-xfusion under same namespace, labels run should be haproxy-ingress. Configure spec as selectors' run should be haproxy-ingress, type should be NodePort. The first port name should be http, its port should be 80, protocol should be TCP, targetPort should be 80 and nodePort should be 32456. The second port name should be https, its port should be 443, protocol should be TCP, targetPort should be 443 and nodePort should be 32567. The third port name should be stat, its port should be 1024, protocol should be TCP, targetPort should be 1024 and nodePort should be 32678.
```
apiVersion: v1
kind: Service
metadata:
  name: ingress-service-xfusion
  namespace: haproxy-controller-xfusion
  labels:
    run: haproxy-ingress
spec:
  type: NodePort
  selector:
    run: haproxy-ingress
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 32456
    - name: https
      protocol: TCP
      port: 443
      targetPort: 443
      nodePort: 32567
    - name: stat
      protocol: TCP
      port: 1024
      targetPort: 1024
      nodePort: 32678
      ```
