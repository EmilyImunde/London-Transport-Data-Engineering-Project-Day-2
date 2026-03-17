which files were used in the Spark workflow
Answer:
1. Raw data files
stations.csv
lines.csv
journeys.json
boroughs.csv
zones.csv
2. src- spark_pipeline.py


what transformations were performed
Answer: 
New reports were made with the following data
Final Transport Report -overll trasport system data
Line Delay - to show which line had most delays, filtered from highest to least
Borough Passengers- to show which borrow had most passangers filtered from highest number of passangers to least



what joins were performed
Answer:
join(stations_df.alias("s"), col("j.station_id") == col("s.station_id"), "inner")
.join(lines_df.alias("l"), col("j.line_id") == col("l.line_id"), "inner")
.join(boroughs_df.alias("b"), col("s.borough_id") == col("b.borough_id"), "left")
.join(zones_df.alias("z"), col("s.zone_id") == col("z.zone_id"), "left")



what output files were created
Answer:
i) Transport report
ii)Line delay report
iii) Borough passagers report
iv) Top stations report


what felt similar to Day 1
Answer:
The output data is similar to that of day one especially the table Final Transport Report. The data is comparable to that of ETL and ELT pipeline, similar coloumns and structure for all 3 reports


what felt different from Day 1
Answer:
The tables were automatically produced without inputing the data into different files. The process felt quite automated. Also one did not have to manually create a database in postgre SQL like the case of and ETL pipeline. Furthermore, when it came to preview of data, the preview tables were automatically produced, without having to use mutiple commands in portgres to preview data like in the ELT pipeline.The end result felt easily attainable as: Spark can clean and transform data in a scalable style, it Spark can join multiple datasets together
and can perform aggregations for reporting.