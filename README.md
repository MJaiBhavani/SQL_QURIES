# SQL_QURIES
select all tables to see the data 
```
SELECT * FROM power_bi.web_traffic_final;
```
```
SELECT * FROM power_bi.device_lookup;
```
```
SELECT * FROM power_bi.geo_lookup;
```
```
SELECT * FROM power_bi.source_lookup;
```
Count of Total sessions 
```
select count(*) as Total_session  from web_traffic_final;
```
Total page views per session in minutes 
```
select sum(page_views_per_session) as Total_session  from web_traffic_final;
```

Total Time in hrs 
```
select sum(duration)/3600 total_hours from web_traffic_final;
```
countof sessions per Month
```
select month(date_col) , count(session_id) from web_traffic_final
group by month(date_col) ;
```
countof sessions per Day of week 
```
select dayname(date_col) , count(session_id) , sum(page_views_per_session) from web_traffic_final
group by dayname(date_col) ;
```
Total hours spend on web based on Source type 
- refferal
- email
- social
- other
- paid

```
select  Source_Type , sum(duration)/3600 as Total_hours 
from web_traffic_final as wf 
natural join 
source_lookup
Group by Source_Type 
order by Total_hours;
```
Total hours spend on web based on Device_type like 
- mobile
- laptop
- desktop
- tablet
- other
```
select device_type , sum(duration)/3600 as Total_hours
from web_traffic_final as wf 
natural join device_lookup
Group by device_type 
order by Total_hours;
```
Total hours spend on web based on content like
- education
- personal
- gaming
- travel and tourism
- Business

```
select  Content_Segment , sum(duration)/3600 as Total_hours 
from web_traffic_final as wf 
natural join device_lookup
Group by Content_Segment 
order by Total_hours;
```
Total sessions categorized by source campaign and month wise sessions count 
```
select  monthn_name , source_campaign ,Total_sessions
from (select monthname(date_col) as montn_name,
month(date_col)  as month ,
source_campaign  ,
count(session_id) as Total_sessions 
from web_traffic_final
join 
source_lookup
on web_traffic_final.Source_Key = source_lookup.Source_Key
group by  monthname(date_col),source_campaign , month(date_col) 
order by  month(date_col) , total_sessions deSC) as table1 ;
```
Total page views  categorized by Devicetype  and month wise  
```
with cte as 
(select Device_Type , SUM(Page_Views_Per_Session)  as Total_page_views, 
month (date_col) as month ,
monthname(date_col) as month_name
from web_traffic_final
natural join 
device_lookup 
group by Device_Type , month(date_col) , monthname(date_col)
order by month )
select month_name , Device_Type , Total_page_views from cte ;
```
Total sessions based on Region and city 
```
select Location_City ,Location_region, count(Session_Id)  as Total_sessions 
from web_traffic_final as wf 
 join 
 geo_lookup  GL 
 on wf.Location_Key = GL.Location_Key
 group by Location_Region, Location_City
 order by Location_Region;
```
