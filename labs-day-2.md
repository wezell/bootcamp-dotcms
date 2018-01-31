Boxboat Boot Camp Labs
----------------------

5.1: Docker Registry
--------------------

This lab will introduce you to the open source Docker Registry.

1. Run the Registry container:
```
docker container run -d -p 5000:5000 --restart=always --name registry registry:2
```

2. Copy an image from Docker Hub (or build your own):
```
docker image pull alpine:latest
docker image ls
```

3. Tag the image to point to your registry:
```
docker tag alpine:latest localhost:5000/my-alpine
docker image ls
```
Image IDs are the same? What about the size?

4. Remove Alpine image:
```
docker image rm alpine
```
What is the command output? Why is this simply an "untag"?

5. Push the image to your registry:
```
docker push localhost:5000/my-alpine
```

6. Test pulling from your registry:
```
docker image remove localhost:5000/my-alpine
docker image pull localhost:5000/my-alpine
```
What if you pull before you remove?

5.2: GitLab Docker Compose
--------------------------

This lab will introduce the Docker Compose tool  
We will spin up GitLab using Docker Compose  

Navigate to docker-compose-lab/gitlab  
execute docker-compose up -d  

5.3: WordPress Docker Compose
-----------------------------

This lab will help us create compose files  
We will launch WordPress using Docker Compose  

Navigate to docker-compose-lab/wordpress  
Create a docker-compose.yml file with the following configuration:  

```
networks:   
	wordpress-net  

volumes:  
	db-data
	wordpress-data  

Database container  
	service name: db  
	image: mysql:5.7  
	volumes: db-data:/var/lib/mysql  
	network: wordpress-net  
	environment variables:  
		MYSQL_ROOT_PASSWORD: wordpress
      	        MYSQL_DATABASE: wordpress
      	        MYSQL_USER: wordpress
      	        MYSQL_PASSWORD: wordpress

WordPress container
	service name: wordpress
	depends on: db
	image: wordpress:latest
	volumes: wordpress-data:/var/www/html
	network: wordpress-net
	ports: 8001:80
	environment variables:
		WORDPRESS_DB_HOST: db:3306
      	        WORDPRESS_DB_PASSWORD: wordpress
```
