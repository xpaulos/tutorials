
# Run and connect to ubuntu container

`docker run -it ubuntu /bin/bash`

# Observe the running container
`docker ps -a`

# Check the available local images
`docker image ls`

# Pull and image 
`docker pull <name>:<tag>`
`docker pull nginx`

# Run the image in a Container
`docker run nginx -p 8084:80`

# Stop the containers
`docker stop < container name >`

# Delete the container
`docker rm <container>`

# Delete the images
`docker rmi <image>`



# Create a personal network

`docker network create my-app-network`


# Create a volume
`docker volume create my-database-data`


# Create your own mysql with Docker
```
docker run -d \
  --name my-database \
  -n my-app-network
  -e MYSQL_ROOT_PASSWORD=12341234 \
  -v my-database-data:/var/lib/mysql \
  mysql:latest
```
  
  
# Login to Dockerhub
` docker login my-registry.example.com `

# Tag Image to dockerhub
  
` docker tag my-local-image:latest my-registry.example.com/my-repo/my-local-image:latest `

# Push Image to dockerhub
  
` docker push my-registry.example.com/my-repo/my-local-image:latest `

