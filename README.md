### TODO

- [ ] Create a service
- [ ] Create a kubernetes manifest
- [ ] Create sub document to explain in details
  - [ ] creating a docker file
  - [ ] docker registry
  - [ ] kubectl, kuberntes, aks, pods, services, load balancer,



# Your first DotNet Service on AKS

This article will help you setup your first service on AKS. It does not intend to provide an in-depth knowledge on the tech-stack. Idea is, that once you have it working you can play around, experiment, eplore and enhance to learn the concepts in depth or to create a prototype.



We will create a simple service and talk about basics of docker and kubernetes. You need no prior experience with Docker/Kubernetes or Azure for this.



In this article we focus on just creating a simple web service. There is another article that demonstrates creating a streaming server with gRPC and Websockets here https://github.com/dotnet-school/dotnet-streaming-aks.



### Pre-requisite

- **.NET 5 SDK**

  > We will use .NET5 boileplates to create a sample service which we will run on AKS.
  >
  > Download and install from  https://dotnet.microsoft.com/download/dotnet/5.0

- **Docker Desktop** 

  > To build and publish images to docker repository. 
  >
  > Download and install from https://www.docker.com/products/docker-desktop.

- **Docker Hub Acccount**

  > For this tutorial we will use Docker Hub as our Docker registry. You can use anything else like Azure Container Registry, but its recommended to use docker hub to help you follow along the steps in this article.
  >
  > Create your account here : https://hub.docker.com/signup

- **Kubectl**

  > To run command against kubernetes cluster on AKS.
  >
  > Download and install from : https://kubernetes.io/docs/tasks/tools/

- **Azure Portal Account**

  > Signup to create you azure account here https://signup.azure.com/signup

- **Azure CLI**

  > To be able to connect to azure via cli. You can skip this and use Azure cloud shell instead. But is recommended to help you follow along the steps in this article.
  >
  > Download and install from https://docs.microsoft.com/en-us/cli/azure/install-azure-cli



### Steps

- [***Create your service***](#create-first-service)

  > Use .NET boilerplate to create a service. 





<a name="create-first-service"></a>

### Create a service with .net5

You can create the service using visual studio. Please ensure you keep the folder strucutre as below : 

- <repository>
  - HelloWorldService
    - HelloWorldService.csproj

For this article we will use the CLI to create a new service.

```bash
# Create a directory for the project
mkdir dotnet-first-aks-service
  
# Initialize a git repo
git init

# Create a .gitignore file
dotnet new gitignore
  
# Create a web api .net5 using boileplate
dotnet new webapi -o HelloWorldService

# Run your service 
dotnet run --project HelloWorldService/HelloWorldService.csproj
```



Now open url https://localhost:5001/WeatherForecast in browser to ensure our service is up and running.

