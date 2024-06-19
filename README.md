# Techaxis-JenkinsCICD
# Build the Jenkins BlueOcean Docker Image (or pull and use the one I built)
 
docker build -t myjenkins-blueocean:2.414.2 . <br>
IF you are having problems building the image yourself, you can pull from my registry (It is version 2.332.3-1 though, the original from the video) <br>
docker pull devopsjourney1/jenkins-blueocean:2.332.3-1 && docker tag devopsjourney1/jenkins-blueocean:2.332.3-1 myjenkins-blueocean:2.332.3-1 <br>

# Create the network 'jenkins'
docker network create jenkins

# Run the Container
# MacOS / Linux
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.414.2

# Windows
docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.414.2

#  Get the Password
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword

# Connect to the Jenkins
https://localhost:8080/
