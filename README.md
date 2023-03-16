# Wordpress Nginx docker-compose.yml
Deploying wordpresss with docker-compose.yml file

This is a Docker Compose file that defines several services to run a WordPress website with SSL certificates using Nginx as a reverse proxy and Certbot for automatic certificate generation and renewal. 
    ## The nginx service:
        Builds an Nginx container from a Dockerfile located in the ./nginx directory.
        Sets several environment variables to be used in the container, including the COMPOSE_PROJECT_NAME and DOMAIN variables.
        Uses an env_file to load additional environment variables from a file named config.env.
        Mounts several volumes to the container, including two volumes named certbot_certs and wpsite_certbot that are external to this Compose file.
        Exposes ports 80 and 443 to the host machine.
        Restarts the container automatically if it stops unexpectedly.
        Depends on the wordpress service.
    ## The certbot service:
        Builds a Certbot container from a Dockerfile located in the ./certbot directory.
        Uses an env_file to load environment variables from config.env.
        Mounts several volumes to the container, including two volumes named certbot_certs and wpsite_certbot that are external to this Compose file.
        Depends on the nginx service.
    ## The cron service:
        Builds a Cron container from a Dockerfile located in the ./cron directory.
        Sets an environment variable named COMPOSE_PROJECT_NAME.
        Mounts several volumes to the container, including the host machine's Docker socket and the current directory (./) as read-only.
        Depends on the nginx and certbot services.
    ## The db service:
        Uses an env_file to load environment variables from config.env.
        Pulls the latest MariaDB image from Docker Hub.
        Mounts a volume named db_data to store the database files.
        Restarts the container automatically if it stops unexpectedly.
    ## The wordpress service:
        Depends on the db service.
        Uses an env_file to load environment variables from config.env.
        Pulls the latest WordPress image from Docker Hub.
        Mounts two volumes, one named wordpress_data to store WordPress files and another located in the current directory (./wordpress) to copy WordPress files to the container.
        Restarts the container automatically if it stops unexpectedly.

Finally, the Compose file defines five volumes, two of which (nginx_ssl and certbot_certs) are external to this Compose file and must be created separately The wpsite_certbot volume is created by this Compose file and is mounted to the Certbot container to store SSL certificates. The db_data and wordpress_data volumes are mounted to the db and wordpress services, respectively, to store database and WordPress files.
