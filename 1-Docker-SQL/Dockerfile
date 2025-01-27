FROM python:3.9

RUN pip install pandas

WORKDIR /app
COPY pipeline.py pipeline.py

ENTRYPOINT [ "python", "pipeline.py" ]

## For Linux
# docker run -it \
#     -e POSTGRES_USER="root" \
#     -e POSTGRES_PASSWORD="root" \
#     -e POSTGRES_DB="ny_taxi" \
#     -p 5433:5432 \
#     -v $(pwd)/ny_taxi_postgress_data:/var/lib/postgresql/data \
#     postgres:17-alpine

## For Windows
# docker run -it `
#     -e POSTGRES_USER="root" `
#     -e POSTGRES_PASSWORD="root" `
#     -e POSTGRES_DB="ny_taxi" `
#     -p 5433:5432 `
#     -v "A:\Repositories\Data-Engineering-Zoomcamp\1-Docker-SQL\ny_taxi_postgress_data:/var/lib/postgresql/data" `
#     postgres:17-alpine
