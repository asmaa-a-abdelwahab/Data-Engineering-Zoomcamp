# Comprehensive Guide to Docker and PostgreSQL

## Why Should We Care About Docker?

1. **Local Experiments**:  
   Docker enables you to create isolated environments for testing and development. This allows developers to run experiments locally without affecting the host system or other projects.

2. **Integration Tests (CI/CD)**:  
   In Continuous Integration/Continuous Deployment pipelines, Docker containers ensure that applications run consistently in different environments by packaging them with all their dependencies.

3. **Reproducibility**:  
   Docker ensures that an application behaves the same way on every machine. By using Docker images, you can replicate the same environment across development, testing, and production.

4. **Running Pipelines on the Cloud**:  
   Tools like AWS Batch and Kubernetes leverage Docker to run pipelines efficiently in the cloud. This enables scalability and simplifies deploying workloads in distributed environments.

5. **Spark**:  
   Docker can package and deploy Apache Spark applications in a portable, consistent environment, simplifying big data processing and enabling deployment across various infrastructures.

6. **Serverless (AWS Lambda, Google Functions)**:  
   Docker images are often used in serverless frameworks to package code and dependencies. This allows developers to write serverless functions in any language and run them reliably on cloud services.

---

## Building Docker Images

1. **Building an Image with Docker**:
   - Use a `Dockerfile` to define the build steps.
   - Example command to build an image:
     ```bash
     docker build -t test:pandas .
     ```

2. **Running the Image**:
   - Run the built image using the command:
     ```bash
     docker run test:pandas
     ```

3. **Adding a Python Script**:
   - Example script (`pipeline.py`) can be added to the Dockerfile for execution within the container.

4. **Passing Arguments**:
   - Update the Dockerfile to accept arguments and rebuild/run the container as needed.

---

## Running PostgreSQL in Docker

1. **Starting PostgreSQL**:
   ```bash
   docker run -it \
       -e POSTGRES_USER="root" \
       -e POSTGRES_PASSWORD="root" \
       -e POSTGRES_DB="ny_taxi" \
       -p 5433:5432 \
       -v A:\Repositories\Data-Engineering-Zoomcamp\1-Docker-SQL\ny_taxi_postgress_data:/var/lib/postgresql/data \
       postgres:13
   ```

2. **Testing Connection**:
   - Use a database client to connect to the Postgres instance running on port 5433.
   - Default credentials:
     - User: `root`
     - Password: `root`

---

## Working with Jupyter Notebooks

1. **SQLAlchemy**:
   - Install compatible versions:
     ```bash
     pip install pandas==2.2.2 sqlalchemy==2.0.35
     ```

2. **Using pgcli**:
   - Show tables:
     ```bash
     \dt
     ```
   - Describe a table:
     ```bash
     \d table_name
     ```
   - Count rows:
     ```sql
     SELECT COUNT(*) FROM table_name;
     ```

---

## Connecting pgAdmin and PostgreSQL

1. **Run pgAdmin in Docker**:
   - Example command:
     ```bash
     docker run -it \
         -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
         -e PGADMIN_DEFAULT_PASSWORD="root" \
         -p 8080:80 \
         dpage/pgadmin4
     ```

2. **Create a Docker Network**:
   ```bash
   docker network create pg_network
   ```

3. **Connect PostgreSQL and pgAdmin**:
   - Run PostgreSQL in the network:
     ```bash
     docker run --net=pg_network -e POSTGRES_USER="root" -e POSTGRES_PASSWORD="root" -d postgres:13
     ```
   - Register PostgreSQL in pgAdmin using `pgdatabase` as the hostname.

---

## Using Docker-Compose

1. **docker-compose.yaml Example**:
   ```yaml
   version: '3.8'
   services:
     postgres:
       image: postgres:13
       environment:
         POSTGRES_USER: root
         POSTGRES_PASSWORD: root
         POSTGRES_DB: ny_taxi
       ports:
         - "5433:5432"
     pgadmin:
       image: dpage/pgadmin4
       environment:
         PGADMIN_DEFAULT_EMAIL: admin@admin.com
         PGADMIN_DEFAULT_PASSWORD: root
       ports:
         - "8080:80"
       depends_on:
         - postgres
   ```

2. **Running Docker-Compose**:
   - Start all services:
     ```bash
     docker-compose up
     ```
   - Stop services:
     ```bash
     docker-compose down
     ```

---

## SQL Refresher

1. **Ingest Taxi Zones Data**:
   - Download the `taxi_zones.csv` file and ingest it into PostgreSQL.
   - Example Jupyter commands:
     ```python
     import pandas as pd
     from sqlalchemy import create_engine

     engine = create_engine("postgresql://root:root@localhost:5433/ny_taxi")
     df = pd.read_csv("taxi_zones.csv")
     df.to_sql("taxi_zones", engine, if_exists="replace")
     ```

2. **Perform SQL Queries**:
   - Join `taxi_zones` and `yellow_taxi_data`:
     ```sql
     SELECT *
     FROM taxi_zones tz
     JOIN yellow_taxi_data ytd
     ON tz.LocationID = ytd.PULocationID;
     
