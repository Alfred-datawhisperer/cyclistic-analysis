# cyclistic-analysis

create table anual(ride_id varchar(50),
 rideable_type varchar(50),
 started_at datetime,
 ended_at datetime,
 start_station_name varchar(100),
 start_station_id varchar(100),
 end_station_name varchar(100),
 end_station_id varchar(100), 
 start_lat double precision,
 start_lng double precision,
 end_lat double precision, 
 end_lng double precision,
 member_casuaal varchar(50) );


load data local infile 'C:/Users/FUTECH COMPUTER/Desktop/portfolio project 1/cyclistic_sql/anualdata.CSV'
into table anual
fields terminated by ','
enclosed by '"'
lines terminated by '\r\n'
ignore 1 rows;

SELECT count(*) as total_number_of_records from anual;

select member_casuaal, count(*) as num_of_users 
from anual
group by member_casuaal
order by num_of_users desc
;

select member_casuaal

select rideable_type, count(*) as ride_usage
from anual
group by rideable_type
order by ride_usage desc
;

SELECT rideable_type, member_casuaal, count(*) as ride_usage
from anual
group by rideable_type,member_casuaal
order by ride_usage desc
;


select start_station_name,start_station_id, count(*) as most_frequent_station
from anual
group by start_station_id, start_station_name
order by most_frequent_station desc
limit 10
;

select end_station_name,end_station_id, count(*) as most_frequent_station
from anual
group by end_station_id, end_station_name
order by most_frequent_station desc
limit 10



select concat( count(*),' ', "users picked bikes at the" ,' ', start_station_name ,' ',  "station, and dropped off the bikes at the" , ' ' ,end_station_name) as overview_of_most_prefared_pickup_and_dropoff_point
from anual
group by start_station_name,end_station_name
order by overview_of_most_prefared_pickup_and_dropoff_point desc
limit 10;


select member_casuaal, sec_to_time(avg(time_to_sec(timediff(ended_at,started_at)))) as average_usage_duration from anual
group by member_casuaal
order by  average_usage_duration desc;

SELECT 
    member_casuaal,
    rideable_type,
    SEC_TO_TIME(AVG(TIME_TO_SEC(TIMEDIFF(ended_at, started_at)))) AS average_usage_duration
FROM
    anual
GROUP BY member_casuaal , rideable_type
ORDER BY average_usage_duration DESC;



SELECT 
    member_casuaal,
    rideable_type,
    SEC_TO_TIME(SUM(TIME_TO_SEC(TIMEDIFF(ended_at, started_at) ) ) ) AS average_usage_duration
FROM
    anual
    group by rideable_type, member_casuaal
ORDER BY average_usage_duration DESC;

SELECT 
    member_casuaal,
    rideable_type,
    SEC_TO_TIME(SUM(TIME_TO_SEC(TIMEDIFF(ended_at, started_at) ) ) ) OVER (PARTITION BY member_causal)
FROM
    anual;


select ride_id,
 member_casuaal,
 rideable_type,
 SEC_TO_TIME(TIME_TO_SEC(TIMEDIFF(ended_at, started_at))) AS average_usage_duration from anual;
