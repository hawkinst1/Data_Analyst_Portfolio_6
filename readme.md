**Numbers - Nasa Exoplanets**

I wanted to look at more numerical based data and along with an interst in interstellar space decided to do some data analysis on NASA's Exoplanets dataset (planets outside our solar system)

The What

What am I looking for in this data?
- is there a correlation between distance and brightness?
- is there a correlation between brightness and year of discovery?
- Are all gas giants comparble to our gas giants in size?
- What is the orbital radius compared to our solar systems?
- For planets with a 1 year orbital period are they comparable to earth?
- For planets with a mass_wrt = 1 (same as Earth) is there a difference in radius_wrt?

What am I going to do with the data?
I first clean all the rows with null in SSMS, rather than wasting time doing where column x or column y ....
I found out about dynamic SQL and got the following to work:


DECLARE @TableName NVARCHAR(MAX) = 'exoplanets';
DECLARE @DynamicSQL NVARCHAR(MAX);

SELECT @DynamicSQL = 'DELETE FROM ' + @TableName + ' WHERE ' +
    STUFF((
        SELECT ' OR ' + COLUMN_NAME + ' IS NULL'
        FROM INFORMATION_SCHEMA.COLUMNS
        WHERE TABLE_NAME = @TableName
        FOR XML PATH('')
    ), 1, 4, '');
 EXEC sp_executesql @DynamicSQL;


This script uses the INFORMATION_SCHEMA.COLUMNS system view to dynamically generate the DELETE statement with the appropriate IS NULL conditions for each column. 
The STUFF function is used to remove the initial "OR" from the generated conditions.
counting the remaining rows:

SELECT
COUNT(name) AS 'Planets'
FROM exoplanets 

I have 4765 rows to analyse.

I think extra columns are needed, I want to add a column that is a bool for; if the orbital radius is the similar to Earth's = 1, 
I will allow a positive for any planets with a radius of 0.85 to 1.15. 

The conditional is "IF orbital_radius IS <= 1.15 OR >= 0.85 = 1 ELSE 0, this can be done with the CASE function:

(add the column)
ALTER TABLE exoplanets 
ADD isEarthObrit BIT
(use the case)
(set all to 0)
UPDATE exoplanets
SET isEarthOrbit = 0

(update only if they meeet the conditions)
UPDATE exoplanets
SET isEarthOrbit = 1
WHERE orbital_radius <= 1.15 AND orbital_radius >= 0.85

89 rows fall within the boundary set.

I also noticed that for the planet_type it gives an answer of 'Neptune-like', to keep inline with the other answers I want to change this to 'Ice Giant' instead.

This needs an update in the column 'planet_type' where value = 'Neptune-like':

update exoplanets
set planet_type = 'Ice Giant'
where planet_type = 'Neptune-like'

I then need to put this SQL data into Power BI, import using the SQL Server name and database name.
(note that 1/0 is converted into TRUE/FALSE)











