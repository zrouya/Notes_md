
Azure Kubernetes Service (AKS) est un type de [[Resource Azure|ressource]] Azure permettant de **déployer** des **[[Clusters Kubernetes|clusters Kubernetes]]** au sein du cloud Azure.

Par exemple, une fois la ressource créée, il est possible d'uploader un fichier [[Fichier yaml|YAML]] dans la section "**Workloads**" de la ressource AKS.

Exemple : 
```yaml
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: apache-deployment
spec:
  replicas: 1 
  selector: 
    matchLabels:
      app: apache-instance
  template: 
    metadata: 
      labels:
        app: apache-instance
    spec: 
      containers:
      - name: apache-instance
        image: httpd
        ports:
        - containerPort: 80

------


apiVersion: v1
kind: Service
metadata:
  name: apache-service
spec:
  selector:
    app: apache-instance
  ports:
    - protocol: 'TCP'
      port: 80 
      targetPort: 
  type: LoadBalancer
```
