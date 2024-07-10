# DockerHub_addon
/home/docker/
├── addons/
├── docker-compose.yaml
└── Dockerfile


1. Edit the Dockerfile to use the relative path:
Update the Dockerfile to use the relative path for the addons directory.

dockerfile
Copy code
# Use the official Odoo image as a base
FROM odoo:16.0

# Copy your custom modules into the image
COPY addons /mnt/extra-addons


2. Build the Docker image again:

sh
Copy code
docker build -t anish443/odoo-custom:16.0 .



3. Ensure your docker-compose.yaml file is correct:

It seems your docker-compose.yaml file is already set up correctly, but make sure it looks like this:

yaml
Copy code
version: '2'

services:
  web:
    image: anish443/odoo-custom:16.0
    depends_on:
      - db
    ports:
      - "8070:8069"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - /home/docker/addons:/mnt/extra-addons
    environment:
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_DB=postgres
      - PGUSER=odoo
      - PGPASSWORD=odoo
      - PGHOST=db
      - PGPORT=5432
      - ODOO_DB_HOST=db
      - ODOO_DB_USER=odoo
      - ODOO_DB_PASSWORD=odoo
      - ODOO_DB_PORT=5432

  db:
    image: postgres:12
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
    volumes:
      - odoo-db-data:/var/lib/postgresql/data

volumes:
  odoo-web-data:
  odoo-db-data:



docker login
#use acccess token of dockerhub

#docker build -t anish443/odoo-custom:16.0 .
docker push anish443/odoo-custom:16.0
