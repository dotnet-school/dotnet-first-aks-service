# Creating docker file



**Images**

- An image is a portable **exectable package** (just like of a binary)

- Defined using a **Dockerfile** (with build command)

- It includes everything to run an application 
  - runtime
  - code
  - configuration and env variables
  - libraries

- Defines what the container will be like when launched at runtime




**Container**

- Container is runtime instance of an image
- i.e. it is an image with state or process
- view all running containers on a machine using ```docker ps```



**Docker Deamon (Your Docker Desktop)**

- Manages all docker operations : 
  - **download images** from registry
  - **launch images as containers**
  - **push images** to repository 
  - etc
- **Inovked via HTTP calls**
- docker **cli makes HTTP calls to docker daemon** running locally 
- Also daemon can be running remotely and docker cli can talk to it via http



![daemon](./images/docker-daemon.png) 



