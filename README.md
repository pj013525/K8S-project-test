# Build a Kubernetes Cluster Locally with Minikube

This notes demonstrates a basic Kubernetes workflow using **Minikube**, including deployment, service exposure, scaling, and verification. 

The example uses an Nginx container.

## Tools Used:

- **Minikube** – Started a local Kubernetes cluster for testing and development.
- **kubectl** – Created deployments, exposed services, scaled pods, and inspected logs.
- **Docker** – Used container images (Nginx) for deploying the application.

---

## **Prerequisites**

- Minikube installed
- Kubectl installed
- Windows, Linux, or Mac terminal access

---

## **Steps to install Minikube**
Go to Official Minikube page:
https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2Fchocolatey

I have chosen chocolatey, because mine is windows and i have installed chocolatey already.

Feel free to change the steps accordingly

- choco install minikube

## **Step 1 : Start Minikube**

Start a Minikube cluster:

```powershell
minikube start
```
Now verify the node:
```bash
kubectl get nodes
```
Expected output:

```psql
NAME       STATUS   ROLES    AGE   VERSION
minikube   Ready    master   1m    v1.xx.x
```
## Step 2: Create Deployment

1. Navigate to your project folder:
```bash
cd "D:\Devops Projects\5.k8s-project\k8s-project"
```
2. Create deployment.yaml:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: nginx:latest
        ports:
        - containerPort: 80
```
3. Apply the deployment:
```
kubectl apply -f deployment.yaml
```
4. Verify pods
```
kubectl get pods
```
Expected output:
```sql
NAME                                READY   STATUS    RESTARTS   AGE
myapp-deployment-xxxxxxxxxx-xxxx    1/1     Running   0          1m
myapp-deployment-xxxxxxxxxx-xxxx    1/1     Running   0          1m
```
## Step 3: Expose Deployment as Service
```bash
Create service.yaml:
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30007
```bash
2. Apply the service:
```
kubectl apply -f service.yaml
```

3. Verify service:
```
kubectl get svc
```
**Expected output:**
```sql
NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
myapp-service   NodePort   10.xxx.xxx.xxx  <none>        80:30007/TCP   10s
kubernetes      ClusterIP  10.96.0.1       <none>        443/TCP        18m
```
## Step 4: Access the App

Use the Minikube service

```
minikube service myapp-service
```
Open the displayed URL in your browser, e.g.:
```
http://192.168.49.2:30007
```
## Step 5: Scale Deployment
Increase replicas:
```
kubectl scale deployment myapp-deployment --replicas=4
kubectl get pods
```
You should see 4 running pods now.

## Step 6: Check Logs and Pod Details
- **Get logs for a pod:**
```
kubectl logs <pod-name>
```
- **Describe pod for details:**
```
kubectl describe pod <pod-name>
```

## Step 7: Screenshots

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/7213bcf9-774e-40c3-baeb-17bd1df6cb45" />

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/f2fcdee7-97f6-4142-86e0-a14dd6caf23a" />

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/c8b78e52-37a7-4c2d-81dc-34cfadde9190" />

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/8d364909-de27-40e2-9dd7-49d5517ff480" />

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/926b2404-6163-4b72-a571-033228ba9a83" />

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/b7c3e3a2-f531-4fde-8f28-6e1731e7df1b" />

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/011a0a28-fcdf-4903-9c3e-0e7797927f33" />

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/e683722a-97b1-4e3c-8679-736126d9327d" />

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/a8d2ebab-b935-42d4-8f0c-cc75196f482a" />













