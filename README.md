# Docker-compose cheatsheet

## What is docker compose?
Compose is a tool for defining and running multi-container Docker applications. 

# Deploy a simple project
```
version: !!str 3

services:
  app:
    build: . 
```

# Deploy Mysql & PhpMyAdmin
```
version: !!str 3

services:
  mysql:
    image: mysql:latest
    networks:
      - no_internet
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
    volumes: 
      - ./data:/var/lib/mysql
      


  phpmyadmin:
    image: phpmyadmin:latest
    networks:
      - no_internet
      - internet
    environment:
      - PMA_ARBITRARY=1
    ports:
      - 8000:80
    depends_on:
      - mysql

networks:
  no_internet:
    driver: bridge
    internal: true # No internet access
  internet:
    driver: bridge
```
Note: You should ```export MYSQL_ROOT_PASSWORD=?``` first.

# Use external network
We can add ```external: true``` to that network.
```
networks:
  internet:
    driver: bridge
    external: true
```

# Scale containers (replications)

We can up containers using ```docker-compose up -d --scale app=5```

# Logs

We can use ```docker-compose logs```.


[@dwsclass](https://github.com/dwsclass) dws-ops-008-compose