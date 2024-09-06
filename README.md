# Deploy Artifacts on a Container
This configuration ensures that Jenkins automatically triggers a build when code is pushed to GitHub, and the resulting .war file is deployed to your Tomcat server.

## Setup CI/CD with GitHub, Jenkins, Maven & Docker.
• Setting up the docker Environment.
• Write DockerFile.
• Create an Image and Container on Docker Host.
• Integrate Docker Host with Jenkins.
• Create CI/CD Job on Jenkins to build and deploy on container.

## Jenkins Stages:
To set up a Jenkins pipeline that performs the following actions:
- **Checkout Code**		: Automatically triggers when a change is pushed to GitHub.
- **Build with Maven**	: Compiles and packages the code.
- **Build Docker Image**:
- **Push Docker Image**	:
- **Deploy to Tomcat Container (Docker)**:
- **Copy .war to Docker Container**: Deploys the resulting .war file to a Tomcat server.

## Prerequisites

### Server Installation
- **Jenkins** : Jenkins should be installed and accessible.	[Installation]
- **Maven** : Maven should be installed and accessible from Jenkins [Installation]
- **Docker** : [Installation]





### Error and Solutions: 

- Issue 1 
	```
	ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied
	```

  Solution 
	```
	sudo usermod -aG docker jenkins
	sudo systemctl restart jenkins
	```

- Issue 2 
	```
	failed to read dockerfile: open /var/lib/docker/tmp/buildkit-mount587080174/Dockerfile: no such file or directory
	```

  Solution 
	```
	dockerfilePath = '/var/lib/jenkins/docker/Dockerfile'	//Mention the Complete directory and file name
	chown -R jenkins:jenkins docker/						// Set the Permission for Dockerfile
	docker build -t ${dockerImage} -f ${dockerfilePath} .
	```

- Issue 3
	```
	The push refers to repository [docker.io/library/tomcat1]
	5f70bf18a086: Preparing
	f36fd4bb7334: Waiting
	denied: requested access to the resource is denied
	```

  Solution 
	```
	docker tag firstimage YOUR_DOCKERHUB_NAME/firstimage		// First Tag the Image, then push on Docker Hub
	docker push YOUR_DOCKERHUB_NAME/firstimage
	```