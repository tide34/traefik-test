apiVersion: apps/v1
kind: Deployment
metadata:
  name: profile-3-22
  labels:
    app: nginx-1-3-22
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-1-3-22
  template:
    metadata:
      labels:
        app: nginx-1-3-22
    spec:
      containers:
      - name: nginx-1-3-22
        image: tide34/web-profile-3-22
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: profile-3-22
spec:
  type: NodePort
  selector:
    app: nginx-1-3-22
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
---
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: profile-3-22-default
spec:
  entryPoints:
    - web
  routes:
  - match: Host( `app.abc` ) && PathPrefix( `/profile`)
    kind: Rule
    priority: 322
    services:
      - name: profile-3-22
        port: 80




---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: root-3-22
  labels:
    app: nginx-2-3-22
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-2-3-22
  template:
    metadata:
      labels:
        app: nginx-2-3-22
    spec:
      containers:
      - name: nginx-2-3-22
        image: tide34/web-root-3-22
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: root-3-22
spec:
  type: NodePort
  selector:
    app: nginx-2-3-22
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
---
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: root-3-22-default
spec:
  entryPoints:
    - web
  routes:
  - match: Host( `app.abc` ) 
    kind: Rule
    priority: 322
    services:
      - name: root-3-22
        port: 80
  
