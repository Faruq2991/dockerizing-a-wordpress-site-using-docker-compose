# Dockerizing a WordPress Site Using Docker Compose

This project demonstrates how to set up a WordPress site using Docker Compose, with a MySQL database as the backend.

## Docker Compose File Overview

The following `docker-compose.yml` file defines the setup for the WordPress and MySQL services:

```yaml
version: '3.7'
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: your_mysql_root_password
      MYSQL_DATABASE: your_mysql_database_name
      MYSQL_USER: your_mysql_username
      MYSQL_PASSWORD: your_mysql_password

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8080:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: your_mysql_username
      WORDPRESS_DB_PASSWORD: your_mysql_password
      WORDPRESS_DB_NAME: your_mysql_database_name

volumes:
  db_data:
```

### Explanation

- **version: '3.7'**: Specifies the version of the Docker Compose file format.
- **services**:
  - **db**: Defines the MySQL database service.
    - **image**: Specifies the MySQL image version `5.7`.
    - **volumes**: Persists database data to the host machine.
    - **restart**: Ensures the container restarts automatically if it stops.
    - **environment**: Sets the environment variables for MySQL credentials and database name.
  - **wordpress**: Defines the WordPress service.
    - **depends_on**: Ensures the WordPress service starts after the database service.
    - **image**: Specifies the latest WordPress image.
    - **ports**: Maps port 8080 on your local machine to port 80 in the container.
    - **restart**: Ensures the container restarts automatically if it stops.
    - **environment**: Sets the environment variables for WordPress to connect to the MySQL database.
- **volumes**:
  - **db_data**: Defines a named volume to persist MySQL data.

## Prerequisites

- Docker and Docker Compose installed on your machine.

## Steps to Set Up the WordPress Site

1. **Create a Directory for the Project**: 
   ```bash
   mkdir wordpress-docker && cd wordpress-docker
   ```

2. **Create the `docker-compose.yml` File**: 
   - Copy the above `docker-compose.yml` content into a new file named `docker-compose.yml`.

3. **Run Docker Compose**:
   ```bash
   docker-compose up -d
   ```
   - This command will download the necessary images and start the services in detached mode.

4. **Access the WordPress Site**:
   - Open your browser and navigate to `http://localhost:8080`.
   - Follow the on-screen instructions to complete the WordPress installation.

5. **Stopping the Services**:
   - To stop the services, run:
     ```bash
     docker-compose down
     ```

## Data Persistence

- The MySQL data is stored in a Docker volume (`db_data`), ensuring that your database persists even when the container is stopped or removed.

## Customization

- You can customize the environment variables in the `docker-compose.yml` file to match your desired configuration, such as setting a custom database name, username, and password.

## Conclusion

This setup provides a quick and easy way to deploy a WordPress site with a MySQL backend using Docker Compose. You can use this as a foundation for more advanced WordPress development and deployment.