# Question 1
command used: 
```
docker run -it python:3.12.8 bash
```
and then 
```
pip --version
```
**ANSWER**: 24.3.1

# Question 2
**ANSWER**: postgres:5432

For adding green taxi data, lesson script were used. Code is located at [(https://github.com/add-later/de-zoomcamp/blob/main/01-docker-terraform/docker-sql/upload_data.ipynb)]
# Question 3
up to 1 mile query
```
SELECT COUNT(index) FROM green_taxi_data
WHERE lpep_pickup_datetime >='2019-10-01' 
    AND lpep_pickup_datetime < '2019-11-01'
    AND lpep_dropoff_datetime >= '2019-10-01' 
    AND lpep_dropoff_datetime < '2019-11-01'
    AND trip_distance <= 1;
```
between 1 (exclusive) and 3 (inclusive)
```
SELECT COUNT(index) FROM green_taxi_data 
WHERE lpep_pickup_datetime >='2019-10-01'
    AND lpep_pickup_datetime < '2019-11-01'
    AND lpep_dropoff_datetime >= '2019-10-01'
    AND lpep_dropoff_datetime < '2019-11-01' 
    AND trip_distance > 1
    AND trip_distance <= 3;
```
**ANSWER**: 104,802; 198,924; 109,603; 27,678; 35,189

# Question 4
Longest trip
```
SELECT lpep_pickup_datetime FROM green_taxi_data
WHERE trip_distance = (SELECT MAX(trip_distance) FROM green_taxi_data)
```
**ANSWER**: 2019-10-11
# Question 5
Biggest pickup zones
```
SELECT green_zones."Zone", COUNT(green_taxi_data.index) AS trip_count, SUM(green_taxi_data.total_amount AS total_sum
FROM green_zones
INNER JOIN green_taxi_data 
    ON green_zones."LocationID" = green_taxi_data."PULocationID"
WHERE DATE(green_taxi_data.lpep_pickup_datetime) = '2019-10-18'
GROUP BY green_zones."Zone"
HAVING SUM(green_taxi_data.total_amount) > 13000
ORDER BY trip_count DESC;
```
note that columns starting with capital letter are taken into quotes (") to be processed and found.
**ANSWER**: East Harlem North, East Harlem South, Morningside Heights

# Question 6
```
SELECT green_zones."Zone"
FROM green_zones
INNER JOIN green_taxi_data
	ON green_taxi_data."DOLocationID" = green_zones."LocationID"
WHERE green_taxi_data.lpep_pickup_datetime >= '2019-10-01'
  AND green_taxi_data.lpep_pickup_datetime < '2019-11-01'
  AND green_taxi_data.tip_amount = (SELECT MAX(green_taxi_data.tip_amount)
FROM green_taxi_data
INNER JOIN green_zones
    ON green_taxi_data."PULocationID" = green_zones."LocationID"
WHERE green_taxi_data.lpep_pickup_datetime >= '2019-10-01'
  AND green_taxi_data.lpep_pickup_datetime < '2019-11-01'
  AND green_zones."Zone" = 'East Harlem North')
```
**ANSWER**: JFK Airport

# Question 7
- `terraform init` — Downloading the provider plugins and setting up backend
- `terraform apply` — Generating proposed changes and auto-executing the plan
- `terraform destroy` — Remove all resources managed by terraform`
**ANSWER**: `terraform init, terraform apply -auto-approve, terraform destroy`
