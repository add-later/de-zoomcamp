Tags: #data-engineering

```
FROM python:3.9.1

RUN pip install pandas

WORKDIR /app
COPY pipeline.py pipeline.py

ENTRYPOINT [ "python", "pipeline.py" ]
```

- `WORKDIR` — created directory in image
- `COPY` — copies file from source directory to docker image:`filename in src`—>`filename in image`
- `ENTRYPOINT` or `--entrypoint=` sets default command to execute when container starts. F.e. `docker run -it --entrypoint=bash python:3.9`
```
docker build -t test:pandas
```
- `docker build` is used to create an image from specfied Dockerfile
- `-t` adds name. In our case the name of docker container will be `test`
- `:pandas` specific version
- `.` created image in the current directory 

To run docker (after build):
```
docker run -it \
-e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
-e PGADMIN_DEFAULT_PASSWORD="root" \
-e PGADMIN_CONFIG_PROXY_X_HOST_COUNT=1 \
-e PGADMIN_CONFIG_PROXY_X_PREFIX_COUNT=1 \
-p 8080:80 \
--network=pg-network \
--name pgadmin-2 \
dpage/pgadmin4
```

# docker-compose
It is used to combine multiple docker containers, allowing running one file instead of multiple docker commands
```services:

pgdatabase:

image: postgres:13

environment:

- POSTGRES_USER=root

- POSTGRES_PASSWORD=root

- POSTGRES_DB=ny_taxi

- PGADMIN_CONFIG_PROXY_X_HOST_COUNT=1

- PGADMIN_CONFIG_PROXY_X_PREFIX_COUNT=1

volumes:

- "./ny_taxi_postgres_data:/var/lib/postgresql/data:rw"

ports:

- "5432:5432"

pgadmin:

image: dpage/pgadmin4

environment:

- PGADMIN_DEFAULT_EMAIL=admin@admin.com

- PGADMIN_DEFAULT_PASSWORD=root

ports:

- "8080:80"
```
