# Docker Compose Learning Guide

This Docker Compose file was crafted as part of my experimentation with Docker, primarily for self-learning purposes. Here's a brief guide summarizing what I've discovered along the way:

## Key Learnings:

### Docker Compose Usage

If you want the `docker-compose.yaml` to utilize a specific `Dockerfile`, ensure you specify it within the `compose.yaml` file. Otherwise, Docker will default to using the public images declared in the `.yaml`.

### Building and Running Containers

To build and run the containers in the background, use the following command:

```bash
docker-compose up -d --build
```

In some cases, it might be useful to add `--force-recreate`:

```bash
docker-compose up -d --build --force-recreate
```

To stop the containers, use:

```bash
docker-compose down
```

### Cleaning Up Environment

To clear the environment, launch the following commands:

```bash
docker-compose down
docker system prune --all --force
```

Ensure that `docker system prune` is executed after `docker-compose down` to detect and remove containers.

### Managing Docker Volumes

List Docker volumes:

```bash
docker volume ls
```

Remove all Docker containers, even if in use:

```bash
docker volume rm $(docker volume ls -qf dangling=true)
```

### Executing SQL Scripts Inside Docker Container

To execute SQL scripts inside a Docker mysql data container, use:

```bash
docker exec -i db_container mysql -uroot -pmyAwesomeStrongPassword mysql < wp_codecrab.sql
```



---

## Docker Compose.yaml Nodes Explanation:

### 1. Database Service (`db`):

**Container Name:** `db_container`  
**Image:** MySQL version 8.0.35

**Volumes:**  
- `db_data:/var/lib/mysql` for persistent data storage  
- `./0_db_dump.sql:/docker-entrypoint-initdb.d/0_db_dump.sql` for initializing the database with dump files.  
- `./1_user.sql:/docker-entrypoint-initdb.d/1_user.sql` for additional database initialization.

**Restart:** Always restart the container if it exits.  
**Environment:** Load environment variables from `.env` file and set `MYSQL_ROOT_PASSWORD`.  
**Networks:** Connect to the `wpNetwork`.

### 2. PhpMyAdmin Service (`phpmyadmin`):

**Container Name:** `phpMyAdmin_container`  
**Depends On:** Ensure the `db` service is running before starting PhpMyAdmin.  
**Image:** Latest PHPMyAdmin version.  
**Restart:** Always restart the container if it exits.  
**Ports:** Map host port 80 to container port 80.  
**Environment:** Load environment variables from `.env` file and set connection details.  
**Networks:** Connect to the `wpNetwork`.

### 3. WordPress Service (`wordpress`):

**Container Name:** `wordpress_container`  
**Depends On:** Ensure the `db` service is running before starting WordPress.  
**Image:** WordPress version 6.1.1.  
**Ports:** Map host port 8000 to container port 80.  
**Restart:** Always restart the container if it exits.  
**Volumes:** Mount the current directory as the WordPress site directory.  
**Environment:** Load environment variables from `.env` file and set database connection details.  
**Networks:** Connect to the `wpNetwork`.

### 4. Networks:

- **wpNetwork:** Custom network for communication between services.

### 5. Volumes:

- **db_data:** Volume for persistent MySQL data storage.



