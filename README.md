# Deploy Artifacts on a Container
This configuration ensures that Jenkins automatically triggers a build when code is pushed to GitHub, and the resulting .war file is deployed to your Tomcat server.

![image](https://github.com/user-attachments/assets/73a750db-35e7-4c32-896d-9348c6390e31)

## Setup CI/CD with GitHub, Jenkins, Maven & Docker.
- Setting up the docker Environment.
- Write DockerFile.
- Create an Image and Container on Docker Host.
- Integrate Docker Host with Jenkins.
- Create CI/CD Job on Jenkins to build and deploy on container.

## Jenkins Stages:
To set up a Jenkins pipeline that performs the following actions:
- **Checkout Code**: Automatically triggers when a change is pushed to GitHub.
- **Build with Maven**: Maven to compile the Java code, run unit tests, and package the application (usually into a .war file for a web application).
- **Build Docker Image**: This stage involves building a Docker image that contains your packaged application (e.g., .war file) and the necessary runtime environment. 
- **Push Docker Image**	:This stage pushes the newly built Docker image to a remote registry so that it can be pulled and used in various environments (e.g., production, staging).
- **Deploy to Tomcat Container (Docker)**:  Pipeline deploys the Docker container running Tomcat. It can either pull the latest Docker image and start the container or update the existing one.
- **Copy .war to Docker Container**: Deploys the resulting .war file to a Tomcat server.

## Prerequisites

### Server Installation
- **Jenkins** : Jenkins should be installed and accessible. **[Installation](https://github.com/manishktomar/bash-scripts)**
- **Maven** : Maven should be installed and accessible from Jenkins **[Installation](https://github.com/manishktomar/bash-scripts)**
- **Docker**: Docker should be installed and accessible from Jenkins **[Installation](https://github.com/manishktomar/bash-scripts)**

### Jenkins Plugin Installation and Setup
- **Credentials**: Credentials for GitHub and Dockerhub should be configured in Jenkins.


## Errors and Solutions: 

#### Issue 1 
	ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied
	
  Solution 
	
	sudo usermod -aG docker jenkins
	sudo systemctl restart jenkins
	

#### Issue 2 
	failed to read dockerfile: open /var/lib/docker/tmp/buildkit-mount587080174/Dockerfile: no such file or directory

  Solution 
	
	dockerfilePath = '/var/lib/jenkins/docker/Dockerfile'			//Mention the Complete directory and file name
	chown -R jenkins:jenkins docker/					// Set the Permission for Dockerfile
	docker build -t ${dockerImage} -f ${dockerfilePath} .
	

#### Issue 3
	The push refers to repository [docker.io/library/tomcat1]
	5f70bf18a086: Preparing
	f36fd4bb7334: Waiting
	denied: requested access to the resource is denied
	
  Solution 
	
	docker tag firstimage YOUR_DOCKERHUB_NAME/firstimage			// First Tag the Image, then push on Docker Hub
	docker push YOUR_DOCKERHUB_NAME/firstimage
	
