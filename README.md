# practice-mode
SELECT * FROM tutorial.crunchbase_companies_clean_date;
select cast(funding_total_usd as varchar), founded_at_clean::varchar AS founded_at_string
FROM tutorial.crunchbase_companies_clean_date;
SELECT CAST(funding_total_usd AS varchar),cast(founded_at_clean AS varchar)
  
  FROM tutorial.crunchbase_companies_clean_date;
  
  SELECT companies.permalink,
       companies.founded_at_clean,
       acquisitions.acquired_at_cleaned,
       acquisitions.acquired_at_cleaned -
         companies.founded_at_clean::timestamp AS time_to_acquisition
  FROM tutorial.crunchbase_companies_clean_date companies
  JOIN tutorial.crunchbase_acquisitions_clean_date acquisitions
    ON acquisitions.company_permalink = companies.permalink
 WHERE founded_at_clean IS NOT NULL;
 SELECT companies.permalink,
       companies.founded_at_clean,
       companies.founded_at_clean::timestamp +
         INTERVAL '1 week' AS plus_one_week
  FROM tutorial.crunchbase_companies_clean_date companies
 WHERE founded_at_clean IS NOT NULL;
 
 
 
 
 
 --Write a query that counts the number of companies acquired within 3 years, 5 years, and 10 years 
 --of being founded (in 3 separate columns). Include a column for total companies acquired as well. 
 --Group by category and limit to only rows with a founding date.


 SELECT companies.category_code,
       COUNT(CASE WHEN acquisitions.acquired_at_cleaned <= companies.founded_at_clean::timestamp + INTERVAL '3 years'
                       THEN 1 ELSE NULL END) AS acquired_3_yrs,
       COUNT(CASE WHEN acquisitions.acquired_at_cleaned <= companies.founded_at_clean::timestamp + INTERVAL '5 years'
                       THEN 1 ELSE NULL END) AS acquired_5_yrs,
       COUNT(CASE WHEN acquisitions.acquired_at_cleaned <= companies.founded_at_clean::timestamp + INTERVAL '10 years'
                       THEN 1 ELSE NULL END) AS acquired_10_yrs,
       COUNT(1) AS total
  FROM tutorial.crunchbase_companies_clean_date companies
  JOIN tutorial.crunchbase_acquisitions_clean_date acquisitions
    ON acquisitions.company_permalink = companies.permalink
 WHERE founded_at_clean IS NOT NULL
 GROUP BY 1
 ORDER BY 5 DESC;
 
 --another dataset
 SELECT *
  FROM tutorial.sf_crime_incidents_2014_01;
  
SELECT incidnt_num,
       date,
       LEFT(date, 10) AS cleaned_date,
       RIGHT(date, 17) AS cleaned_time
  FROM tutorial.sf_crime_incidents_2014_01;
  
  SELECT incidnt_num,
       date,
       LEFT(date, 10) AS cleaned_date,
       RIGHT(date, LENGTH(date) - 11) AS cleaned_time
  FROM tutorial.sf_crime_incidents_2014_01;
 
 --leading, trailing can be used instead of both..leading trim beginning..trailing trim end 
  SELECT location,
       TRIM(both '()' FROM location)
  FROM tutorial.sf_crime_incidents_2014_01;
  
  --positon
  SELECT incidnt_num,
       descript,
       POSITION('A' IN descript) AS a_position
  FROM tutorial.sf_crime_incidents_2014_01;
 --OR
  SELECT incidnt_num,
       descript,
       STRPOS(descript, 'A') AS a_position
  FROM tutorial.sf_crime_incidents_2014_01;

  --substr SUBSTR(*string*, *starting character position*, *# of characters*)
  SELECT incidnt_num,
       date,
       SUBSTR(date, 4, 2) AS day
  FROM tutorial.sf_crime_incidents_2014_01;
  
--Write a query that separates the `location` field into separate fields for latitude and longitude. 
--You can compare your results against the actual `lat` and `lon` fields in the table.  
SELECT location,
       TRIM(leading '(' FROM LEFT(location, POSITION(',' IN location) - 1)) AS lattitude,
       TRIM(trailing ')' FROM RIGHT(location, LENGTH(location) - POSITION(',' IN location) ) ) AS longitude
  FROM tutorial.sf_crime_incidents_2014_01;
  
  --concat
  SELECT incidnt_num,
       day_of_week,
       LEFT(date, 10) AS cleaned_date,
       CONCAT(day_of_week, ', ', LEFT(date, 10)) AS day_and_date
  FROM tutorial.sf_crime_incidents_2014_01;
  
  --Concatenate the lat and lon fields to form a field that is equivalent to the location field. 
  --(Note that the answer will have a different decimal precision.)
  SELECT location,
  CONCAT('(',lat,',',lon,')')AS actual_location
  FROM tutorial.sf_crime_incidents_2014_01;

  --Alternatively, you can use two pipe characters (||) to perform the same concatenation:
  SELECT incidnt_num,
       day_of_week,
       LEFT(date, 10) AS cleaned_date,
       day_of_week || ', ' || LEFT(date, 10) AS day_and_date
  FROM tutorial.sf_crime_incidents_2014_01;
  
   SELECT location,
'('||lat||','||lon||')'AS actual_location
  FROM tutorial.sf_crime_incidents_2014_01;
  
  -- a query that creates a date column formatted YYYY-MM-DD.
     SELECT incidnt_num,
           date,
           SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) || '-' || SUBSTR(date, 4, 2) AS cleaned_date
      FROM tutorial.sf_crime_incidents_2014_01;
  
  --UPPERCASE AND LOWERCASE
  SELECT incidnt_num,
       address,
       UPPER(address) AS address_upper,
       LOWER(address) AS address_lower
  FROM tutorial.sf_crime_incidents_2014_01;
  
  --a query that returns the `category` field,
  --but with the first letter capitalized and the rest of the letters in lower-case.
SELECT incidnt_num,
       category,
       UPPER(LEFT(category, 1)) || LOWER(RIGHT(category, LENGTH(category) - 1)) AS category_cleaned
  FROM tutorial.sf_crime_incidents_2014_01;
  
  --CAST
  SELECT incidnt_num,
       date,
       (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) ||
        '-' || SUBSTR(date, 4, 2))::date AS cleaned_date
  FROM tutorial.sf_crime_incidents_2014_01;
  
  --a query that creates an accurate timestamp using the date and time columns in tutorial.sf_crime_incidents_2014_01. 
  --Include a field that is exactly 1 week later as well.

  
  SELECT incidnt_num,
       (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) ||
        '-' || SUBSTR(date, 4, 2) || ' ' || time || ':00')::timestamp AS timestamp,
       (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) ||
        '-' || SUBSTR(date, 4, 2) || ' ' || time || ':00')::timestamp
        + INTERVAL '1 week' AS timestamp_plus_interval
  FROM tutorial.sf_crime_incidents_2014_01;

--another dataset
  SELECT *
  FROM tutorial.sf_crime_incidents_cleandate;
  
  SELECT cleaned_date,
       EXTRACT('year'   FROM cleaned_date) AS year,
       EXTRACT('month'  FROM cleaned_date) AS month,
       EXTRACT('day'    FROM cleaned_date) AS day,
       EXTRACT('hour'   FROM cleaned_date) AS hour,
       EXTRACT('minute' FROM cleaned_date) AS minute,
       EXTRACT('second' FROM cleaned_date) AS second,
       EXTRACT('decade' FROM cleaned_date) AS decade,
       EXTRACT('dow'    FROM cleaned_date) AS day_of_week
  FROM tutorial.sf_crime_incidents_cleandate;
  
  SELECT cleaned_date,
       DATE_TRUNC('year'   , cleaned_date) AS year,
       DATE_TRUNC('month'  , cleaned_date) AS month,
       DATE_TRUNC('week'   , cleaned_date) AS week,
       DATE_TRUNC('day'    , cleaned_date) AS day,
       DATE_TRUNC('hour'   , cleaned_date) AS hour,
       DATE_TRUNC('minute' , cleaned_date) AS minute,
       DATE_TRUNC('second' , cleaned_date) AS second,
       DATE_TRUNC('decade' , cleaned_date) AS decade
  FROM tutorial.sf_crime_incidents_cleandate;
  
  --a query that counts the number of incidents reported by week. 
  --Cast the week as a date to get rid of the hours/minutes/seconds.
  
  SELECT DATE_TRUNC('week', cleaned_date)::date AS week_beginning,
       COUNT(*) AS incidents
  FROM tutorial.sf_crime_incidents_cleandate
 GROUP BY 1
 ORDER BY 1;
 
 SELECT CURRENT_DATE AS date,
       CURRENT_TIME AS time,
       CURRENT_TIMESTAMP AS timestamp,
       LOCALTIME AS localtime,
       LOCALTIMESTAMP AS localtimestamp,
       NOW() AS now;
       
       SELECT CURRENT_TIME AS time,
       CURRENT_TIME AT TIME ZONE 'PST' AS time_pst;
   
   --a query that shows exactly how long ago each indicent was reported. 
   --Assume that the dataset is in Pacific Standard Time (UTC - 8).    
       SELECT incidnt_num,
       cleaned_date,
       NOW() AT TIME ZONE 'PST' AS now,
       NOW() AT TIME ZONE 'PST' - cleaned_date AS time_ago 
  FROM tutorial.sf_crime_incidents_cleandate;
  
  --coalesce
  SELECT incidnt_num,
       descript,
       COALESCE(descript, 'No Description')
  FROM tutorial.sf_crime_incidents_cleandate
 ORDER BY descript DESC;
 
 --subquery
 SELECT sub.*
  FROM (
        SELECT *
          FROM tutorial.sf_crime_incidents_2014_01
         WHERE day_of_week = 'Friday'
       ) sub
 WHERE sub.resolution = 'NONE';
 
 select * from tutorial.sf_crime_incidents_2014_01;
--a query that selects all Warrant Arrests from the tutorial.sf_crime_incidents_2014_01 dataset,
--then wrap it in an outer query that only displays unresolved incidents. 
SELECT sub.*
      FROM (
            SELECT *
              FROM tutorial.sf_crime_incidents_2014_01
             WHERE descript = 'WARRANT ARREST'
           ) sub
     WHERE sub.resolution = 'NONE';
   --What if you wanted to figure out how many incidents get reported on each day of the week? Better yet,
   --what if you wanted to know how many incidents happen, on average, on a Friday in December? In January?
   --There are two steps to this process: counting the number of incidents each day (inner query),
   --then determining the monthly average (outer query):  
     SELECT LEFT(sub.date, 2) AS cleaned_month,
       sub.day_of_week,
       AVG(sub.incidents) AS average_incidents
  FROM (
        SELECT day_of_week,
               date,
               COUNT(incidnt_num) AS incidents
          FROM tutorial.sf_crime_incidents_2014_01
         GROUP BY 1,2
       ) sub
 GROUP BY 1,2
 ORDER BY 1,2;
 --a query that displays the average number of monthly incidents for each category. 
 --Hint: use tutorial.sf_crime_incidents_cleandate to make your life a little easier.
 
SELECT sub.category,
       AVG(sub.incidents) AS avg_incidents_per_month
  FROM (
        SELECT EXTRACT('month' FROM cleaned_date) AS month,
               category,
               COUNT(1) AS incidents
          FROM tutorial.sf_crime_incidents_cleandate
         GROUP BY 1,2
       ) sub
 GROUP BY 1;
 
 --a query that displays all rows from the three categories with the fewest incidents reported.

SELECT incidents.*,
       sub.count AS total_incidents_in_category
  FROM tutorial.sf_crime_incidents_2014_01 incidents
  JOIN (
        SELECT category,
               COUNT(*) AS count
          FROM tutorial.sf_crime_incidents_2014_01
         GROUP BY 1
         ORDER BY 2
         LIMIT 3
       ) sub
    ON sub.category = incidents.category;
    
    --a query that counts the number of companies founded and acquired by quarter starting in Q1 2012. 
    --Create the aggregations in two separate queries, then join them.
    
      SELECT COALESCE(companies.quarter, acquisitions.quarter) AS quarter,
           companies.companies_founded,
           acquisitions.companies_acquired
      FROM (
            SELECT founded_quarter AS quarter,
                   COUNT(permalink) AS companies_founded
           FROM tutorial.crunchbase_companies
             WHERE founded_year >= 2012
             GROUP BY 1
           ) companies
      
      LEFT JOIN (
            SELECT acquired_quarter AS quarter,
                   COUNT(DISTINCT company_permalink) AS companies_acquired
              FROM tutorial.crunchbase_acquisitions
             WHERE acquired_year >= 2012
             GROUP BY 1
           ) acquisitions
        
        ON companies.quarter = acquisitions.quarter
     ORDER BY 1;
     
-- a query that ranks investors from the combined dataset above by the total number of investments they have made.
    SELECT *
          FROM tutorial.crunchbase_investments_part1;
    
    SELECT investor_name,
       COUNT(*) AS investments
  FROM (
        SELECT *
          FROM tutorial.crunchbase_investments_part1
         
         UNION ALL
        
         SELECT *
           FROM tutorial.crunchbase_investments_part2
       ) sub
 GROUP BY 1
 ORDER BY 2 DESC;
 
 --next
 SELECT duration_seconds,
       SUM(duration_seconds) OVER (ORDER BY start_time) AS running_total
  FROM tutorial.dc_bikeshare_q1_2012;
  
  SELECT start_terminal,
       duration_seconds,
       SUM(duration_seconds) OVER
         (PARTITION BY start_terminal ORDER BY start_time)
         AS running_total
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08';
 --query that shows the duration of each ride as a percentage of the total time 
 --accrued by riders from each start_terminal
 SELECT start_terminal,
       duration_seconds,
       SUM(duration_seconds) OVER (PARTITION BY start_terminal) AS start_terminal_sum,
       (duration_seconds/SUM(duration_seconds) OVER (PARTITION BY start_terminal))*100 AS pct_of_total_time
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08'
 ORDER BY 1, 4 DESC;

SELECT start_terminal,
       start_time,
       duration_seconds,
       ROW_NUMBER() OVER (ORDER BY start_time)
                    AS row_number
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08';
 
 SELECT start_terminal,
       start_time,
       duration_seconds,
       ROW_NUMBER() OVER (PARTITION BY start_terminal
                          ORDER BY start_time)
                    AS row_number
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08';
 
 SELECT start_terminal,
       duration_seconds,
       RANK() OVER (PARTITION BY start_terminal
                    ORDER BY start_time)
              AS rank
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08';
 
 
 --Write a query that shows the 5 longest rides from each starting terminal, ordered by terminal, 
 --and longest to shortest rides within each terminal.
 --Limit to rides that occurred before Jan. 8, 2012.
 
 SELECT *  FROM tutorial.dc_bikeshare_q1_2012;
 
 SELECT *
  FROM (
        SELECT start_terminal,
               start_time,
               duration_seconds AS trip_time,
               RANK() OVER (PARTITION BY start_terminal ORDER BY duration_seconds DESC) AS rank
          FROM tutorial.dc_bikeshare_q1_2012
         WHERE start_time < '2012-01-08'
               ) sub
 WHERE sub.rank <= 5;
 
 --NTILE
 SELECT start_terminal,
       duration_seconds,
       NTILE(4) OVER
         (PARTITION BY start_terminal ORDER BY duration_seconds)
          AS quartile,
       NTILE(5) OVER
         (PARTITION BY start_terminal ORDER BY duration_seconds)
         AS quintile,
       NTILE(100) OVER
         (PARTITION BY start_terminal ORDER BY duration_seconds)
         AS percentile
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08'
 ORDER BY start_terminal, duration_seconds;
 
 --use LAG or LEAD to create columns that pull values from other rows—all you need to do is enter 
 --which column to pull from and how many rows away 
 --you'd like to do the pull. LAG pulls from previous rows and LEAD pulls from following rows:
 SELECT start_terminal,
       duration_seconds,
       LAG(duration_seconds, 1) OVER
         (PARTITION BY start_terminal ORDER BY duration_seconds) AS lag,
       LEAD(duration_seconds, 1) OVER
         (PARTITION BY start_terminal ORDER BY duration_seconds) AS lead
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08'
 ORDER BY start_terminal, duration_seconds;
 

 SELECT start_terminal,
       duration_seconds,
       duration_seconds -LAG(duration_seconds, 1) OVER
         (PARTITION BY start_terminal ORDER BY duration_seconds)
         AS difference
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08'
 ORDER BY start_terminal, duration_seconds;
