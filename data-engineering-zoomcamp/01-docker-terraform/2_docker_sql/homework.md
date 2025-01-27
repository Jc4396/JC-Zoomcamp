## Question 1. Understanding docker first run
Run docker with the python:3.12.8 image in an interactive mode, use the entrypoint bash.

What's the version of pip in the image?

```bash
docker run -it --entrypoint=bash python:3.12.8
```

```bash
pip -V
```

## Question 2. Understanding Docker networking and docker-compose
Given the following docker-compose.yaml, what is the hostname and port that pgadmin should use to connect to the postgres database?

services:
  db:
    container_name: postgres
    image: postgres:17-alpine
    environment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'postgres'
      POSTGRES_DB: 'ny_taxi'
    ports:
      - '5433:5432'
    volumes:
      - vol-pgdata:/var/lib/postgresql/data

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4:latest
    environment:
      PGADMIN_DEFAULT_EMAIL: "pgadmin@pgadmin.com"
      PGADMIN_DEFAULT_PASSWORD: "pgadmin"
    ports:
      - "8080:80"
    volumes:
      - vol-pgadmin_data:/var/lib/pgadmin  

volumes:
  vol-pgdata:
    name: vol-pgdata
  vol-pgadmin_data:
    name: vol-pgadmin_data

The hostname the hostname and port for pgAdmin to connect to Postgres is db:5432

## Question 3. Trip Segmentation Count
During the period of October 1st 2019 (inclusive) and November 1st 2019 (exclusive), how many trips, respectively, happened:

Up to 1 mile
In between 1 (exclusive) and 3 miles (inclusive),
In between 3 (exclusive) and 7 miles (inclusive),
In between 7 (exclusive) and 10 miles (inclusive),
Over 10 miles

```SQL
SELECT 
    CASE 
        WHEN trip_distance <= 1 THEN 'Up to 1 mile'
        WHEN trip_distance > 1 AND trip_distance <= 3 THEN 'Between 1 and 3 miles'
        WHEN trip_distance > 3 AND trip_distance <= 7 THEN 'Between 3 and 7 miles'
        WHEN trip_distance > 7 AND trip_distance <= 10 THEN 'Between 7 and 10 miles'
        ELSE 'Over 10 miles'
    END AS Distance_Range,
    COUNT(1) AS Trip_Count
FROM 
    green_taxi_trips_zones
WHERE 
    lpep_pickup_datetime >= '2019-10-01 00:00:00'
    AND lpep_pickup_datetime < '2019-11-01 00:00:00'
    AND lpep_dropoff_datetime < '2019-11-01 00:00:00'
GROUP BY 
    Distance_Range
```
## Question 4. Longest trip for each day
Which was the pick up day with the longest trip distance? Use the pick up time for your calculations.

Tip: For every day, we only care about one single trip with the longest distance.

```SQL
SELECT 
    CAST(lpep_pickup_datetime AS DATE) AS Pickup_Day,
    MAX(trip_distance) AS Longest_Trip_Distance
FROM 
    green_taxi_trips_zones
GROUP BY 
    CAST(lpep_pickup_datetime AS DATE)
ORDER BY 
    Longest_Trip_Distance DESC
LIMIT 1;
```

## Question 5. Three biggest pickup zones
Which were the top pickup locations with over 13,000 in total_amount (across all trips) for 2019-10-18?

Consider only lpep_pickup_datetime when filtering by date.

```SQL
SELECT 
    pickup_loc, 
    SUM(total_amount) AS total_amount_sum
FROM 
    green_taxi_trips_zones
WHERE 
    lpep_pickup_datetime::date = '2019-10-18'
GROUP BY 
    pickup_loc
HAVING 
    SUM(total_amount) > 13000
ORDER BY 
    total_amount_sum DESC
LIMIT 3;
```

## Question 6. Largest tip

For the passengers picked up in October 2019 in the zone named "East Harlem North" which was the drop off zone that had the largest tip?

Note: it's tip , not trip

We need the name of the zone, not the ID.

```SQL
SELECT
	pickup_loc,
	dropoff_loc,
	MAX(tip_amount) AS Largest_Tip
FROM
	green_taxi_trips_zones
WHERE
	pickup_loc = 'East Harlem North'
	AND lpep_pickup_datetime >= '2019-10-01 00:00:00'
    AND lpep_pickup_datetime < '2019-11-01 00:00:00'
GROUP BY
	pickup_loc,
	dropoff_loc
ORDER BY 
    Largest_Tip DESC
LIMIT 1;
```

## Question 7. Terraform Workflow

Which of the following sequences, respectively, describes the workflow for:

Downloading the provider plugins and setting up backend,
Generating proposed changes and auto-executing the plan
Remove all resources managed by terraform`

Answer: terraform init, terraform apply -auto-approve, terraform destroy