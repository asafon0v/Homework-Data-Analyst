## Задача 1
SELECT name, COUNT(Pass_in_trip.trip) as count FROM Passenger  

LEFT JOIN  Pass_in_trip ON Passenger.id = Pass_in_trip.passenger  

GROUP BY Passenger.id HAVING count >= "1"  

ORDER BY count DESC, name;  


