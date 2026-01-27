
# Run a container with podman
podman run --name basic_httpd -d -p 8088:80/tcp docker.io/nginx
# Check if the container is running
podman ps
# Check if nginx is running
curl http://localhost:8088

# Check the logs
podman logs <container_id>
# Stop the container
podman stop <container_id>
# Check stopped Container
podman ps -a
# Delete containers
podman rm <container_id>
# Search for images
podman search <search_term>

# Pull an image
podman pull docker.io/library/httpd
# Check available local
podman image list or podman images
# Run the image in a container
podman run -d -p 8089:80/tcp docker.io/library/httpd
# Check the running container
podman ps


