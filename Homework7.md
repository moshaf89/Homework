# 7.1 HW Questions 


1. Using EVregistry, Write a query to select the `ModelYear`, `Make`, and `Model` off all of the vehicles in the registry.
SELECT ModelYear, make, Model
FROM EVRegistry
2. Using the EVRegistry table, Write a query that lists all of the unique types of EV's. your reult set should have one column, `ElectricVehicleType`. 
SELECT DISTINCT ElectricVehicleType
FROM EVRegistry
3. Using the EVRegistry, Write a query that shows all of the information on Battery Electric Vehicles (BEV) that are in the registry. 
SELECT *
from EVRegistry
where ElectricVehicleType = 'Battery Electric Vehicle (BEV)'
4. Using the EVRegistry, wirte a query that returns the `Make` and `Model` of all of the EV's that have a BaseMSRP between 20000 and 35000?  
SELECT make, Model, BaseMSRP
from EVRegistry
where BaseMSRP > 20000 AND BaseMSRP < 35000

# 7.2 HW Questions 

1. Using EVRegistry, write a query to find a record  where the `City` attribute is NULL. Return all of the available columns. 
SELECT *
FROM EVRegistry
where city is NULL
2. Write a query to find the `make`, `model`, and `ElectricVehicleType` where the VIN number has  that ends in '3E1EA1J'.
SELECT make, model, ElectricVehicleType, VIN
FROM EVRegistry
where VIN like '%3E1EA1J'
3. Select the `ModelYear`, `make`, `model`, `ElectricVehicleType`, and `range` of the Tesla vehicles or cheverolet vehicles in the registry. Order the result set by Make and Model year in from newest to oldest. 
SELECT model, make, ModelYear, ElectricVehicleType, ElectricRange
from EVRegistry
where Make is 'TESLA' or make is 'CHEVROLET'
order by make, ModelYear DESC
4. Using EVCharging, Write a query to find out how many many times those stations were used. Order them by the most used to the least used and limit the output to 5 records. 
SELECT stationId, Count(*)
FROM evCharge
GROUP BY stationId
ORDER BY Count(*) DESC
LIMIT 5
5.  Using EVCharging, For the folks who charged longer than 0.5 hours, show the min and max of the charging time for each user. Your output columns should be `userid`, `minTime`, and `maxTime`. Order this result set by the last two columns respectively.
SELECT userId, min(chargeTimeHrs), max(chargeTimeHrs)
FROM evCharge
WHERE chargeTimeHrs > .5
group by userId
order by min(chargeTimeHrs), max(chargeTimeHrs)



# Before moving on with the rest of the questions please set up the new database
1. in SQLlite close any open DB
2. file----> Open Database
3. Choose SavvyCoders_SQL_chargeDB.db from the resource repository from this section in the curriculum
4. Make sure that you have 5 tables: 
    - dimDay 
    - dimFacility
    - dimUser
    - factCharge
    - EVCharging


# 7.3 HW Questions

1. Using EVCharging, Which day of the week has the highest average charging time? Round the answer to 2 decimal points.
SELECT weekday, round(avg(chargeTimeHrs),2)
FROM evCharging
group by weekday
order by round(avg(chargeTimeHrs),2) DESC
2. Using, EV charging, Find the total power consumed from charging EV's by each User. Return the `userId` and name the calculated column, `totalPower`. Round the answer to 2 deciaml points and list the out put in highest to lowest order. Limit the order to the top 15 users. 
select userId, round(sum(kwhTotal),2) as 'totalPower'
from EVCharging
group by userId
order by totalPower DESC
LIMIT 15
3. Using dimfacility and factCharge, write a query to find out which type of facility (GROUP BY) has the most amount of charging stations. Return `type Facility` and name the calculated column `numStation`. Order the result set from highest to lowest number of charging stations.
SELECT b.typeFacility, Count(*) as 'numStation'
FROM factCharge a 
INNER JOIN dimFacility b 
ON a.facilityID = b.FacilityKey
GROUP BY b.FacilityKey
ORDER BY 2 DESC
4. In your own words, Briefly explain Primary Keys and Foreign Keys. 
-primary key is column from a table that makes it unique... foreign key is the primary key of different table 
5. Using EV Charging, For the folks who charged longer than one hour, show the min and max of the charging time for each user. Your output columns should be `userid`, `minTime`, and `maxTime`. Order this result set by the last two columns respectively. HINT: USE `HAVING`
SELECT userId, min(chargeTimeHrs) as 'minTime', max(chargeTimeHrs) as 'maxTime'
FROM EVCharging
GROUP BY userId
HAVING min(chargeTimeHrs) > 1
ORDER BY 2,3