# Azure_MERN_Deployment

# MERN Microservices deployment using AKS

This application has 3 microservices, 2 for the backend and 1 for the frontend.
```
tree --filelimits 3 MERN_Microservices_EKS_Deployment/
```
```
MERN_Microservices_EKS_Deployment/
├── README.md
├── backend
│   ├── README.md
│   ├── helloService  
│   └── profileService  
└── frontend 
```
## Table of Contents
1. [Set up the Azure Environment](#set-up-the-azure-environment)
   - 1.1 [Set Up Azure CLI](#set-up-azure-cli)
   - 1.2 [Create the Resource Group](#create-the-resource-group)
   - 1.3 [Create the Kubernetes Cluster](#create-the-kubernetes-cluster)
2. [Prepare the MERN Application](#prepare-the-mern-application)
   - 2.1 [Containerize the MERN Application](#containerize-the-mern-application)



## Set up the Azure Environment

### Set Up Azure CLI
Download and install the Azure CLI on Windows
```
https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli
```
After successful installation

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/7c32dfb6-1dcb-462a-a03f-b17e74c14510)

### Creating the resource group
 - Go to the Azure Portal and click on "Create a resource" in the menu on the left.
 - Look for "Resource group" and choose it from the search results.
 - Press the "Create" button.
 - Provide a name that hasn't been used before for your resource group, like "microservice_deployment".
 - Select a region where you want your resource group to be located, such as East US.
 - Click on "Review + Create" and then hit "Create" to finalize creating the resource group.

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/12ca557c-61e3-44e1-bb72-26bc8c54735e)



### Create the Kubernetes Cluster
 - In the Azure Portal, click on “Create a resource” again.
 - Search for “Kubernetes Service” and select “Kubernetes Service (AKS)” from the results.
 - Click the “Create” button to start the AKS creation wizard.


### Basics
 - In the “Basics” tab of the AKS creation wizard:
 - Pick your Azure subscription.
 - Choose the resource group you previously made (named "microservice_deployment").
 - Give your AKS cluster a unique name, like "Mern_Deployment".
 - Select the region where you want your AKS cluster to be, such as East US.
 - Pick the version of Kubernetes you want to use, for example, 1.26.6.
 - If you're practicing or doing development/testing, choose a preset configuration for your cluster, like "Dev/Test", which is optimized for these tasks.
 - Specify the availability zones where your cluster nodes will be placed for better resilience.
 - There are two pricing options for the managed Kubernetes control plane. Choose the one that fits your needs.
 - Decide how you want the cluster to be upgraded when new AKS and Kubernetes versions are released. For instance, you can select "Enable with Patch" for automatic upgrades.
 - For authentication and authorization, you can opt for using local accounts with Kubernetes RBAC. This means a native Kubernetes RBAC will be managed within your AKS cluster.
 - Click "Next: Node Pools" to continue.

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/f3827960-06d1-494f-9e65-f110a3fe2d23)

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/14e6e73a-606f-45ac-a32f-38703a734892)

### Node Pool

 - You can choose to add or change node pools based on the requirements of your applications.
 - Decide on the number of nodes you need, the size of the virtual machines (VMs), and any other settings required for your node pool.
 - Once you've finished configuring your node pool, click on "Next: Networking" to proceed.
   
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/eef18acf-9a3b-499a-b6ec-58aefc1b5b53)

### Networking

 - Configure the networking settings for your AKS cluster. The default settings are usually sufficient for most use cases.
   
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/a415b6ad-12ce-4191-b514-eda17071315d)

### Integration

 - Configure integrations with Azure services and features.
 - You can enable Azure Container Registry integration, Azure Policy, and more.

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/3843b7f1-7c59-412c-abd3-09650cec8333)

### Review + Create

 - Review all the configuration settings to ensure they are correct.
 - If everything looks good, click the “Create” button to start the provisioning of the AKS cluster.

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/939c1e93-12c7-4770-9aa3-6f9f1b970710)

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/204db509-9fe2-4c4e-80a7-b8123c6cd0cd)

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/37b2cc69-07c8-4c2b-b417-b3071ce380d4)

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/c8076295-b35e-47c9-bdc2-c6f38c9d15a0)


## Prepare the MERN Application

### Containerize the MERN Application


## Backend Service

```
tree --filelimit 3 backend/
```
```
backend/
├── README.md
├── helloService  
└── profileService  
```

## Hello Microservice
```
cd helloService
```
Inside the helloservice create a .env file for dynamic hosting the application at available port
```
nano .env
PORT=$PORT
```
Inside the helloservice create a Dockerfile which will help to containerize the hello microservice
```
nano Dockerfile
```
```
FROM node:18
WORKDIR /
COPY . /
RUN npm install
ENV PORT 3001
CMD ["node", "index.js","3001"]
```
Build the docker image
```
docker build -t be_hello_svc:latest .
```
Lets try to test the helloservice container
```
docker run -it -e PORT=3001 -p 3001:3001 be_hello_svc:latest
```
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/f3259634-4106-4074-bdd0-a9cfe7ae27b7)

```
docker tag be_hello_svc:latest sayanalokesh/hello_service_adarsh:latest
docker push adarsh321/hello_service_adarsh:latest
```

## Profile Microservice
```
cd profileService
```
Inside the profileservice create a .env file for dynamic hosting the application at available port and pass the MONGO URL
```
nano .env
PORT=$PORT
MONGO_URL=$MONGO_URL
```
Inside the profileservice create a Dockerfile which will help to containerize the profile microservice
```
nano Dockerfile
```
```
FROM node:18
WORKDIR /
COPY . /
RUN npm install
ENV PORT 3002
ENV MONGO_URL MONGO_URL
CMD ["node", "index.js","3002"]
```
Build the docker image
```
docker build -t be_profilesvc .
```
Lets try to test the profileservice container
```
docker run -it -e MONGO_URL="your mongo url" -p 3002:3002 be_profilesvc:latest
```
Lets push the docker images to docker hub
```
docker tag be_profilesvc:latest sayanalokesh/profile_service_adarsh:latest
docker push sayanalokesh/profile_service_sayanalokesh:latest
```

# Frontend Micro Service

In Frontend microservice it will connect to both the backend microservice helloservice and profile service.

```
cd frontend
```
Inside the frontend microservice create a .env file for dynamically changing the backend urls for both hello microservice and profile microservice

```
nano .env
```
```
REACT_APP_API_URL=$REACT_APP_API_URL
REACT_APP_API_HELLO=$REACT_APP_API_HELLO
```

Now let's containerise the frontend microservice
```
FROM node:18
WORKDIR /
COPY . /
ENV REACT_APP_API_URL $REACT_APP_API_URL
ENV REACT_APP_API_HELLO $REACT_APP_API_HELLO
RUN npm install
CMD ["npm", "run", "start"]
```

Lets build the Docker Image of the Frontend Microservice
```
docker build -t fe_svc .
```
Lets test the Docker container with the environment variables
```
docker run -it -e REACT_APP_API_HELLO=http://<ec2 public ip>:3001 -e REACT_APP_API_URL=http://<ec2 public ip>:3002/fetchUser -p 3000:3000 fe_svc
```

Now push the image to the Dockerhub

```
docker tag fe_svc:latest sayanalokesh/frontend_service_sayanalokesh:latest
docker push sayanalokesh/frontend_service_adarsh:latest
```

## Deployment in AKS

Installing kubectl cli on windows
```
choco install kubernetes-cli
```
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/579eb691-fb8d-48d3-803a-b0eedac1ee49)


```
az aks get-credentials --resource-group microservice_deployment --name Mern_deployment
```
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/fe9cceab-6ec3-4805-8381-f0642973b1bb)

Let's test it 

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/3c74b5b0-303b-43f1-af92-8bd222a80881)
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/669b7413-65a8-4948-bc1f-3571c499d687)

## Deploying Hello Microservice

Imperative approach-
```
kubectl create deployment hello --image=public.ecr.aws/c3w1m1q2/hello_service_sayanalokesh --replicas=3 --port=3001
```
Declarative approach
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: hello
  name: hello
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
      - image: sayanalokesh/hello_service_sayanalokesh:latest
        name: hello-microservice
        ports:
        - containerPort: 3001
```
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/aaa0231b-627b-480e-a924-15dbbdd86104)
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/e10508ae-5cbb-49f8-a21d-9d339575b0ac)

Now creating service to expose the deployment

Imperative approach-
```
kubectl expose deployment hello --port=3001 --target-port=3001 --type=LoadBalancer --name=hello-svc
```
Declarative approach
```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: hello
  name: hello-svc
spec:
  ports:
  - port: 3001
    protocol: TCP
    targetPort: 3001
  selector:
    app: hello
  type: LoadBalancer
status:
  loadBalancer: {}
```
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/f32fcd61-ccf1-465a-851b-33df75385efc)
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/ac581ea5-4d5b-478f-b40e-7a8a51918913)
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/5fe3d48c-3949-47bc-a4f2-3253fb904957)

## Deploying Profile Microservice

This Microservice is using credential of MongoDB database to handle that will be using secrets

Creating Secret to handle the MONGO DB credientials
```
kubectl create secret generic mongo-secret --from-literal=MONGO_URL=" your mongo db url"
```
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/cd6662c7-3e64-419f-9683-444317995623)
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/b38753c4-4384-4b72-ad26-cfba1746cec3)

Deployment file of Profile Microservice
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: profile-service-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: profile-service
  template:
    metadata:
      labels:
        app: profile-service
    spec:
      containers:
        - name: profile-service-container
          image: sayanalokesh/profile_service_sayanalokesh:latest
          ports:
            - containerPort: 3002
          env:
            - name: MONGO_URL
              valueFrom:
                secretKeyRef:
                  name: mongo-secret
                  key: MONGO_URL
```
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/4f220164-c5f6-4749-824a-7f6552870eac)
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/c5dc526b-0368-43e9-a92b-82f2589f5653)

Now expose the Deployment to Loadbalancer service
```
kubectl expose deployment profile-service-deployment --type=LoadBalancer --port=3002 --target-port=3002 --name=profile-service
```
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/ab310de6-551e-48b5-8942-fd5058cbd19a)
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/2c8de7c2-2f2b-435b-9104-083d5023f36d)

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/0f0c7be5-9b84-4032-8f3e-70fa6ed612f5)


## Deploying Frontend Microservice

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: frontend-container
          image: sayanalokesh/frontend_service_sauanalokesh:latest
          ports:
            - containerPort: 3000  
          env:
            - name: REACT_APP_API_URL
              value: "http://your- profile-loadbalancer-service-dns"
            - name: REACT_APP_API_HELLO
              value: "http://your-hello-loadbalancer-service-dns"
```

```
kubectl expose deployment frontend-deployment --type=LoadBalancer --port=3000 --target-port=3000 --name=frontend-service
```
![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/5757d163-e915-4c39-b79c-55dc8efcaf63)

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/675fbbac-6d82-465d-abde-99ddb7d8a2ff)

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/b6864a78-a412-4906-b77e-8a16a5d491ac)

![image](https://github.com/AdarshIITDH/MERN-deployment-on-AKS/assets/60352729/72d8660c-1486-4576-ad04-c5da25e5d7c8)
