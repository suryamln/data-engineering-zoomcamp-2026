### **Nama : Surya Maulana**
---

#### Question 1. What's the version of pip in the python:3.13 image? (1 point)
- Answer : 25.3
- Solution :
1. Run docker using command:
```bash
docker run -it --rm -p 8888:8888 -v "${pwd}:/app" --entrypoint=bash python:3.13
```
2. After entering the Docker environment, enter the command below to check the pip version
```bash
pip -V
```

#### Question 2. Given the docker-compose.yaml, what is the hostname and port that pgadmin should use to connect to the postgres database? (1 point)

```yaml
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
```

- Answer : db:5432
- Solution: 
<img width="940" height="550" alt="image" src="https://github.com/user-attachments/assets/48c6d6d9-4e9b-4ac6-bbec-91279eef64f3" />

#### Question 3. For the trips in November 2025, how many trips had a trip_distance of less than or equal to 1 mile? (1 point)
- Answer : 8,007
- Solution :
```SQL
SELECT count(1) AS "Jumlah Trip"
FROM public.green_taxi_trips_november_2025 
WHERE lpep_pickup_datetime >= '2025-11-01 00:00:00' 
AND lpep_pickup_datetime < '2025-12-01 00:00:00'
AND trip_distance <= 1
```

#### Question 4. Which was the pick up day with the longest trip distance? Only consider trips with trip_distance less than 100 miles. (1 point)
- Answer : 2025-11-14
- Solution :
```SQL
SELECT lpep_pickup_datetime AS "Tanggal Pickup", trip_distance AS "Jarak Trip"
FROM public.green_taxi_trips_november_2025 
WHERE trip_distance 
IN 
(SELECT MAX(trip_distance) FROM public.green_taxi_trips_november_2025 
WHERE trip_distance < 100)
```

#### Question 5. Which was the pickup zone with the largest total_amount (sum of all trips) on November 18th, 2025? (1 point)
- Answer : East Harlem North
- Solution :
```SQL
SELECT TZ."Zone" AS "Pickup Zone", COUNT(TZ."Zone") AS "Jumlah Pickup" 
FROM public.green_taxi_trips_november_2025 GTT
JOIN public.taxi_zone TZ
ON GTT."PULocationID" = TZ."LocationID"
WHERE 
DATE(lpep_pickup_datetime) = '2025-11-18'
GROUP BY "Pickup Zone"
ORDER BY "Jumlah Pickup" DESC
```

#### Question 6. For the passengers picked up in the zone named "East Harlem North" in November 2025, which was the drop off zone that had the largest tip? (1 point)
- Answer : Yorkville West
- Solution :
```SQL
SELECT 
TZ_PU."Zone" AS "Pickup Zone", 
TZ_DO."Zone" AS "Dropoff Zone", 
MAX(GTT.tip_amount) AS "Jumlah Tip" 

FROM public.green_taxi_trips_november_2025 GTT

JOIN public.taxi_zone TZ_PU
ON GTT."PULocationID" = TZ_PU."LocationID"
JOIN public.taxi_zone TZ_DO
ON GTT."DOLocationID" = TZ_DO."LocationID"

WHERE
TO_CHAR(GTT."lpep_pickup_datetime", 'YYYY-MM') = '2025-11' AND
TZ_PU."Zone" = 'East Harlem North'

GROUP BY "Pickup Zone", "Dropoff Zone"
ORDER BY "Jumlah Tip"  DESC
```

#### Question 7. Which of the following sequences describes the Terraform workflow for: 1) Downloading plugins and setting up backend, 2) Generating and executing changes, 3) Removing all resources? (1 point)
Answer : terraform init, terraform apply -auto-approve, terraform destroy





