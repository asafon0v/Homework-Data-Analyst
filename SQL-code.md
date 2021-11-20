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
