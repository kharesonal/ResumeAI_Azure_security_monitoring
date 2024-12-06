# ResumeAI

# MEAN Application Deployment with Azure Services

ResumeAI is a MEAN stack application designed to streamline the process of building and managing resumes. This project demonstrates the deployment of the application using Azure services, ensuring scalability and performance. It includes the setup of a comprehensive monitoring dashboard to track metrics, performance, and health of the application components.

## Table of Contents
- Project Structure
- Getting Started
     - Backend Setup
     - Frontend Setup
- Environment Variables
- Deployment to AKS
- Monitoring
- CI/CD with Jenkins
    - Setup Jenkins Pipeline

## Project structure

This project includes two main parts:

- Backend: Node.js and Express application connected to Azure Cosmos DB.
- Frontend: Angular application served through Azure Static Web Apps.

## Getting Started

### Prerequisites

Before starting, ensure you have the following:

- An Azure account (sign up here).
- Node.js and npm installed on your local machine.
- Angular CLI installed for building the frontend.
- Azure CLI installed for managing Azure resources from the command line.
- Git for version control.

### Clone the Repository

```
git clone https://github.com/UnpredictablePrashant/ResumeAI.git
cd ResumeAI
```

## Backend Setup

1. Navigate to the backend directory

    `
     cd ResumeBuilderBackend
    `

3. Create an environment file (.env) with the necessary configuration variables. Refer to Environment Variables for more details.

4. To run the backend locally, you can use Docker. Create a Dockerfile with the following contents:
   
```
Copy code
FROM node:18
WORKDIR /app
COPY package*.json ./
COPY . .
RUN npm install
RUN npm install -g typescript
RUN npm run build
RUN apt-get update && apt-get install -y chromium
EXPOSE 4292
CMD [ "npm", "start" ]
```
4. Build and run the Docker container:

```
docker build -t resumeai-backend .
docker run -p 4292:4292 --env-file .env resumeai-backend
```
## Frontend Setup

1. Navigate to the frontend directory:
   
`
cd ResumeBuilderAngular
`

2. Create an environment file (.env) with the necessary configuration variables. Refer to Environment Variables for more details.
   
3. To run the frontend using Docker, create a Dockerfile with the following contents:
   
 ```
Copy code
FROM node:18
WORKDIR /app
COPY . .
RUN npm install -f
EXPOSE 4200
CMD ["/bin/sh", "-c", "npm run generate-proxy-config && npm start"]
```

4. Build and run the Docker container:
   
 ```
  docker build -t resumeai-frontend .
  docker run -p 4200:4200 --env-file .env resumeai-frontend
 ```

5. Environment Variables:
   
For both backend and frontend, you need to configure environment variables as follows:

 Backend .env Variables
- JWT_SECRET_KEY: A secret key for signing JWT tokens. Example: MYREALLYSECRETKEY
- MONGO_URL: MongoDB connection URL. Example: mongodb+srv://...
- OPENAI_KEY: OpenAI API key. Example: YOUR_OPENAI_API_KEY
- GMAIL_USER: Gmail address used by Nodemailer to send resumes. Example: youremail@gmail.com
- GMAIL_PASS: Password or App Password for the Gmail user.
- FRONT_END: URL of the frontend. Example: http://192.168.1.96:4200

 Frontend .env Variables
- API_TARGET: URL of the backend API. Example: http://192.168.1.96:4292

## Deployment to AKS

1. Create Azure Kubernetes Service (AKS) cluster:
   
```
 # Create resource group once login to AZ using `az login`
 az group create --name <AKS_RESOURCE_GROUP> --location <AKS_LOCATION>
 # Create AKS cluster
 az aks create --resource-group <AKS_RESOURCE_GROUP> --name <AKS_CLUSTER_NAME> --node-count 2 --node-vm-size Standard_DS2_v2 --generate-ssh-keys
```

2. Configure kubectl to use your Azure Kubernetes Service (AKS) cluster:
   
```
kubectl config use-context {AKS_CLUSTER_NAME}
```

3. Create Kubernetes deployment and service files for both frontend and backend using helm.
   
`
 helm create ResumeAI
`

4. Navigate to the helm directory, Apply the deployment:
   
```
cd ResumeAI
helm repo add stable https://charts.helm.sh/stable
helm repo update
helm install resumeai ./resumeai
```

5.Verify the deployment:

```
kubectl get pods
kubectl get services
```

## Monitoring Dashboard

To ensure the health and performance of your deployed MEAN application, you can set up a monitoring dashboard using Azure Monitor. These services will allow you to track key metrics, diagnose issues, and gain insights into your application.

### Set Up Azure Monitor Dashboard

**1. Create a New Dashboard**:

- In the Azure Portal, search for Azure Monitor and select Dashboards.
- Click on New Dashboard and give it a name (e.g., ResumeAI Monitoring Dashboard).

![image](https://github.com/user-attachments/assets/74db26ff-a883-4ec7-ba66-3e48ff06bd45)

**2. Add Monitoring Metrics:**

Application Metrics: To monitor the backend performance:

- In the Dashboard settings, click Add a tile and choose Metrics.
- Select Application Insights as the source, then choose the appropriate metric (e.g., requests, failed requests, dependencies).

![Screenshot 2024-12-06 150859](https://github.com/user-attachments/assets/4004106b-d354-4273-a024-8f33ac230fad)


![Screenshot 2024-12-06 150520](https://github.com/user-attachments/assets/cf25c19c-fed4-4ef4-b195-895d97335116)


App Service Metrics: For tracking App Service health and usage:

- Click Add a tile and select Metrics.
- Select App Service as the resource, and then choose metrics like CPU Usage, Memory Usage, Response Time, etc.

![Screenshot 2024-12-06 151434](https://github.com/user-attachments/assets/6c7c988f-e86d-409a-91da-3eb555c6a43a)

  
Customize the Dashboard:

- Rearrange the tiles as needed to prioritize key metrics.
- Save the dashboard to quickly access it whenever you need to check the application’s performance.

**Step 3: Key Metrics to Monitor**

The following key metrics are important for keeping track of your application's performance and health:

- App Service Metrics:

 - CPU Usage
 - Memory Usage
 - Response Time
 - Requests/Failures
 - Active Connections

![Screenshot 2024-12-06 152020](https://github.com/user-attachments/assets/c3487891-9b19-4f8b-a422-2e77af66e74e)

## Output

![Screenshot 2024-12-06 150239](https://github.com/user-attachments/assets/98d2234e-3e92-47f4-849d-b6f8a70635b8)







