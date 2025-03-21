![Screenshot from 2025-03-20 11-46-20](https://github.com/user-attachments/assets/a5c8dcbe-6e10-444e-800f-b46809bb0a41)Installing and setting up Kubernetes Minikube..

Update system packages and Install dependencies:

sudo apt update && sudo apt upgrade -y

sudo apt install -y curl wget apt-transport-https conntrack socat
![Screenshot from 2025-03-20 11-45-57](https://github.com/user-attachments/assets/70773c26-c0e0-4698-9ed0-5a8466bd55b7)
![Screenshot from 2025-03-20 11-46-11](https://github.com/user-attachments/assets/ba4685ba-fca9-4d38-9a34-1786d0eb1c7f)
Install Docker, Kubectl and Minikube ..

Install Docker..

sudo apt install -y docker.io

sudo systemctl start docker

sudo systemctl enable docker

docker --version

![Screenshot from 2025-03-20 11-46-20](https://github.com/user-attachments/assets/4c204925-64bb-4fae-acb9-4e2083c0fbbe)

Install Kubectl..

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl

sudo mv kubectl /usr/local/bin/

kubectl version --client

Install Minikube..

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube version

![Screenshot from 2025-03-20 11-51-20](https://github.com/user-attachments/assets/37125fa6-e7b0-449e-b177-b56cb8b943e6)

![Screenshot from 2025-03-20 11-51-35](https://github.com/user-attachments/assets/10742e22-2ecb-4e0c-bcd8-bfb7d7c560d7)

![image](https://github.com/user-attachments/assets/8fe0603f-3677-4ea4-acaf-f87265506558)

Config file codes : 

1) Dockerfile :

  # Use an official Node.js runtime as a base image
FROM node:18

# Set the working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package.json ./
RUN npm install

# Copy the rest of the application
COPY . .

# Expose port 3000 and start the app
EXPOSE 3000
CMD ["npm", "start"]

2) nginx-deployment.yaml :

   apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: sanjai4334/docker:latest
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 8080
 3) service.yaml :

    apiVersion: v1
kind: Service
metadata:
  name: my-app
  namespace: default
spec:
  type: NodePort  # Ensures external access via a specific port
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80       # Service port inside the cluster
      targetPort: 80  # The container's port
      nodePort: 30391   # Externally accessible port
      
