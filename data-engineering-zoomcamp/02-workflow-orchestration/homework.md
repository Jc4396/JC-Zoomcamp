# Question 1
Within the execution for Yellow Taxi data for the year 2020 and month 12: what is the uncompressed file size (i.e. the output file yellow_tripdata_2020-12.csv of the extract task)?

![alt text](<Screenshot 2025-02-04 at 20.48.01.png>)

# Question 2
What is the rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution?

-green_tripdata_2020-04.csv

# Question 3
How many rows are there for the Yellow Taxi data for all CSV files in the year 2020?

SELECT * 
FROM `endless-beach-421319.de_zoomcamp.yellow_tripdata` 
WHERE filename >= 'yellow_tripdata_2020-01.csv' 
AND filename <= 'yellow_tripdata_2020-12.csv' 

# Question 4
How many rows are there for the Green Taxi data for all CSV files in the year 2020?

SELECT * 
FROM `endless-beach-421319.de_zoomcamp.green_tripdata` 
WHERE filename >= 'green_tripdata_2020-01.csv' 
  AND filename <= 'green_tripdata_2020-12.csv' 

# Question 5
How many rows are there for the Yellow Taxi data for the March 2021 CSV file?

SELECT * 
FROM `endless-beach-421319.de_zoomcamp.yellow_tripdata` 
WHERE filename = 'yellow_tripdata_2021-03.csv'

# Question 6
Add a timezone property set to America/New_York in the Schedule trigger configuration