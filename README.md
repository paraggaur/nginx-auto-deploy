# nginx-auto-deploy

Project structure:
```
.
├── ansible
│   └── Dockerfile.
├── docker-compose.yml
├── nginx
│   └── Dockerfile
└── README.md


[_docker-compose.yaml_](docker-compose.yaml)
```
version: "3"
services:
  nginx:
    image: ubuntu/nginx:1.18-21.10_beta
    container_name: nginx-deploy
    hostname: nginx-deploy
    build:
      context: ./nginx
      dockerfile: Dockerfile
    ports:
      - 80:80
      - 443:443
  ansible:
    container_name: ansible-deploy
    hostname: ansible-deploy
    build: 
      context: ./ansible
      dockerfile: Dockerfile
      
## Deploy with docker-compose

```
$ docker-compose up -d


## Expected result

Listing containers must show two containers running and the port mapping as below:

# docker ps
CONTAINER ID   IMAGE                          COMMAND                  CREATED       STATUS          PORTS                                                                      NAMES
bd9e33ec68fc   nginx-auto-deploy_ansible      "/usr/lib/systemd/sy…"   7 hours ago   Up 51 minutes                                                                              ansible-deploy
2c6ff266a2b2   ubuntu/nginx:1.18-21.10_beta   "/docker-entrypoint.…"   7 hours ago   Up 51 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp   nginx-deploy
