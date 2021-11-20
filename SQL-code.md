## Задача 1
SELECT name, COUNT(Pass_in_trip.trip) as count FROM Passenger  
LEFT JOIN  Pass_in_trip ON Passenger.id = Pass_in_trip.passenger  
GROUP BY Passenger.id HAVING count >= "1"  
ORDER BY count DESC, name;  
## Задача 2
SELECT DISTINCT TIMEDIFF(   
(SELECT end_pair FROM Timepair WHERE id=4),  
(SELECT start_pair FROM Timepair WHERE id=2)  
) as time  
FROM Timepair;  
## Задача 3
SELECT DISTINCT Rooms.*  
FROM Rooms  
JOIN  Reservations  
    ON Rooms.id = Reservations.room_id  
WHERE WEEK(start_date, 1) = 12 AND YEAR(start_date) = 2020;  
## Задача 4
SELECT classroom  
FROM Schedule  
GROUP BY classroom  
HAVING COUNT(classroom) =  
    (SELECT COUNT(classroom)   
     FROM Schedule   
     GROUP BY classroom  
     ORDER BY COUNT(classroom) DESC   
     LIMIT 1)  
## Задача 5
WITH distinct_dates as (SELECT DISTINCT date from income_o), intervals as (SELECT date dt1, LEAD(date) OVER (ORDER BY date) as dt2 FROM distinct_dates)   
SELECT coalesce(sum(out), 0), dt1, dt2 FROM intervals LEFT JOIN Outcome_o on Outcome_o.date > dt1 and Outcome_o.date <= dt2 WHERE dt2 is not null GROUP BY dt1, dt2;  
## Задача 6
WITH   
helper_one AS  
    (SELECT   
         ROW_NUMBER() OVER (ORDER BY DATE) number_one,  
         name,  
         date,  
         NTILE(2) OVER (ORDER BY DATE) group_one  
     FROM   
         Battles b  
    ),  
helper_two AS   
     (SELECT    
          *,  
          ROW_NUMBER() OVER (PARTITION BY group_one ORDER BY DATE) number_two  
      FROM   
          helper_one  
     )  
SELECT   
    max(iif(group_one = 1, number_one, null)),  
    max(iif(group_one = 1, name, null)),  
    max(iif(group_one = 1, date, null)),  
    max(iif(group_one = 2, number_one, null)),  
    max(iif(group_one = 2, name, null)),  
    max(iif(group_one = 2, date, null))  
FROM  
    helper_two  
GROUP BY   
    number_two  

