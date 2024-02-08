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

