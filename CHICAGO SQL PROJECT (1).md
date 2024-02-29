In this PROJECT, I will employ SQL for the purpose of conducting an analysis and comprehension of three distinct datasets pertinent to Chicago. These datasets, accessible through the City of Chicago's Data Portal, encompass:

1. Socioeconomic Indicators in Chicago:
This dataset encompasses six selected socioeconomic indicators of public health significance and a "hardship index" for each Chicago community area, spanning the years 2008 to 2012.

2. Chicago Public Schools:
This dataset comprises comprehensive school-level performance data utilized in the creation of CPS School Report Cards for the 2011-2012 school year. The dataset is sourced from the City of Chicago's Data Portal.

3. Chicago Crime Data:
Reflecting reported incidents of crime (excluding murders where data exists for each victim) from 2001 to the present in the City of Chicago, excluding the most recent seven days.

These datasets have been imported and organized into distinct tables within a database established in Microsoft SQL Server Management Studio. The subsequent analysis will be executed through the application of SQL queries, with connectivity established between my SQL server database and Jupyter notebook. The objective is to address specific inquiries derived from the datasets.


1.	Find the total number of crimes recorded in the CRIME table.
2.	List community area names and numbers with per capita income less than 11000.
3.	List all case numbers for crimes involving minors?(children are not considered minors for the purposes of crime analysis)
4.	List all kidnapping crimes involving a child?
5.	List the kind of crimes that were recorded at schools. (No repetitions)
6.	List the type of schools along with the average safety score for each type.
7.	List 5 community areas with highest % of households below poverty line
8.	Which community area is most crime prone? Display the coumminty area number only
9.	Use a sub-query to find the name of the community area with highest hardship index
10.	Use a sub-query to determine the Community Area Name with most number of crimes?

Setting Up the connection Environment

To start, I need to set up my Python environment to facilitate the connection to  my SQL Server. This involves installing the necessary Python libraries that enable database communication.

```python
#Installing the required python packages
!pip install pyodbc

```

    Requirement already satisfied: pyodbc in c:\users\user\anaconda3\lib\site-packages (4.0.34)
    


```python
import pyodbc 
```


```python
# Setting the connection parameters
server = 'DESKTOP-HJKSALQ\SQLEXPRESS'
database = 'Chicago_projects'
username = '#####'
password = '#######

# Creating a connection string
conn_str = "DRIVER={SQL Server};SERVER=DESKTOP-HJKSALQ\SQLEXPRESS;DATABASE=Chicago_projects;UID=****;PWD=####"
```
Establishing the Connection

With the environment set up, I will  establish a connection between my Jupyter Notebook and my SQL Server database. This involves creating a connection string and using it to connect to the database. A connection string contains the information needed to connect to the database. It usually includes the database driver, the server name, database name, and authentication details

```python
# Establishing a connection
# Establishing a connection
try:
    connection = pyodbc.connect(conn_str)
    print("Connection successful!")
    
    # Perform additional operations here if needed

except Exception as e:
    print(f"Error connecting to the database: {e}")

```

    Connection successful!
    
Wih connection established with my database, i will proceed to answering the question listed at the beginning of this project

```python
# 1 Find the total number of crimes recorded in the CRIME table.

import pyodbc



# Create a cursor
cursor = connection.cursor()

# SQL query with an alias for the count
query = "SELECT COUNT(*) AS total_crimes FROM dbo.[ChicagoCrimeData$];"

# Execute the query
cursor.execute(query)

# Fetch the result
result = cursor.fetchone()

# Display the result with a heading
print(f"Total Crime: {result.total_crimes}")

# Close the cursor
cursor.close()

# Close the connection
connection.close()

```

    Total Crime: 533
    
Total Crime is 533

```python

# Establishing a connection
try:
    connection = pyodbc.connect(conn_str)
    print("Connection successful!")
    
    # Perform additional operations here if needed

except Exception as e:
    print(f"Error connecting to the database: {e}")

```

    Connection successful!
    
#2 List community area names and numbers with per capita income less than 11000

```python
# Create a cursor
cursor = connection.cursor()

# SQL query to list community area names and numbers with per capita income less than 11000
query1 = """SELECT "COMMUNITY_AREA_NAME", "COMMUNITY_AREA_NUMBER" 
            FROM [dbo].[ChicagoCensusData$]  -- Adjusted table name and schema
            WHERE "PER_CAPITA_INCOME" < 11000;"""

# Execute the query
cursor.execute(query1)

# Fetch the results
results = cursor.fetchall()

# Print the header
print("Community Area Name\tCommunity Area Number")
print("------------------------------------------")

# Print the results row by row
for row in results:
    community_area_name, community_area_number = row
    print(f"{community_area_name}\t\t\t{community_area_number}")



```

    Community Area Name	Community Area Number
    ------------------------------------------
    West Garfield Park			26.0
    South Lawndale			30.0
    Fuller Park			37.0
    Riverdale			54.0
    
3. List all case numbers for crimes involving minors?(children are not considered minors for the purposes of crime analysis)

```python
# Create a cursor for executing SQL queries
cursor = connection.cursor()

# Your SQL query
query2 = "SELECT DISTINCT CASE_NUMBER FROM [dbo].[ChicagoCrimeData$] WHERE DESCRIPTION LIKE '%MINOR%'"

# Execute the query
cursor.execute(query2)

# Fetch the results
results = cursor.fetchall()

# Display the results
for row in results:
    print(row.CASE_NUMBER)


```

    HK238408
    HL266884
    
4. List all kidnapping crimes involving a child?

```python
cursor = connection.cursor()

query3 = "SELECT DISTINCT CASE_NUMBER, PRIMARY_TYPE, DATE, DESCRIPTION FROM [dbo].[ChicagoCrimeData$] WHERE PRIMARY_TYPE='KIDNAPPING';"

cursor.execute(query3)

results = cursor.fetchall()

# Print the header
print("CASE_NUMBER    PRIMARY_TYPE        DATE       DESCRIPTION")
print("----------------------------------------------------------------")

# Fetch the results
for row in results:
    CASE_NUMBER, PRIMARY_TYPE, DATE, DESCRIPTION = row
    print(f"{CASE_NUMBER}\t{PRIMARY_TYPE}\t{DATE}\t{DESCRIPTION}")

```

    CASE_NUMBER    PRIMARY_TYPE        DATE       DESCRIPTION
    ----------------------------------------------------------------
    HN144152	KIDNAPPING	2007-01-26 00:00:00	CHILD ABDUCTION/STRANGER
    
5. List the kind of crimes that were recorded at schools. (No repetitions)

```python
cursor = connection.cursor()

query4 = "SELECT DISTINCT(PRIMARY_TYPE), LOCATION_DESCRIPTION FROM [dbo].[ChicagoCrimeData$]\
          WHERE LOCATION_DESCRIPTION LIKE '%SCHOOL%';"
cursor.execute(query4)

# Fetch the results
results = cursor.fetchall()

# Print the header
print("PRIMARY_TYPE      LOCATION_DESCRIPTION")
print("------------------------------------------")

# Keep track of unique PRIMARY_TYPE values
unique_primary_types = set()

for row in results:
    PRIMARY_TYPE, LOCATION_DESCRIPTION = row
    
    # Check if PRIMARY_TYPE is already printed
    if PRIMARY_TYPE not in unique_primary_types:
        # Print the values for the first occurrence of PRIMARY_TYPE
        print(f"{PRIMARY_TYPE}\t\t\t{LOCATION_DESCRIPTION}")
        
        # Add PRIMARY_TYPE to the set to avoid repetition
        unique_primary_types.add(PRIMARY_TYPE)

```

    PRIMARY_TYPE      LOCATION_DESCRIPTION
    ------------------------------------------
    ASSAULT			SCHOOL, PUBLIC, GROUNDS
    BATTERY			SCHOOL, PUBLIC, BUILDING
    CRIMINAL DAMAGE			SCHOOL, PUBLIC, GROUNDS
    CRIMINAL TRESPASS			SCHOOL, PUBLIC, GROUNDS
    NARCOTICS			SCHOOL, PUBLIC, BUILDING
    PUBLIC PEACE VIOLATION			SCHOOL, PRIVATE, BUILDING
    
6. List the type of schools along with the average safety score for each type.

```python
cursor = connection.cursor()

query5 = """SELECT [Elementary, Middle, or High School], AVG(SAFETY_SCORE) AS AVERAGE_SAFETY_SCORE
FROM [dbo].[ChicagoPublicSchools$]
GROUP BY [Elementary, Middle, or High School];"""

cursor.execute(query5)

results = cursor.fetchall()

# Print the header
print("ELEMENTARY, MIDDLE, OR HIGH SCHOOL         AVERAGE_SAFETY_SCORE")
print("----------------------------------------------------------------")

# Fetch the results
for row in results:
    ELEMENTARY_MIDDLE_OR_HIGH_SCHOOL, AVERAGE_SAFETY_SCORE = row
    print(f"{ELEMENTARY_MIDDLE_OR_HIGH_SCHOOL}\t{AVERAGE_SAFETY_SCORE}")

```

    ELEMENTARY, MIDDLE, OR HIGH SCHOOL         AVERAGE_SAFETY_SCORE
    ----------------------------------------------------------------
    ES	49.52038369304557
    HS	49.62352941176471
    MS	48.0
    
7. List 5 community areas with highest % of households below poverty line

```python
# Create a cursor
cursor = connection.cursor()


sql_query = '''
SELECT TOP 5 COMMUNITY_AREA_NAME, PERCENT_HOUSEHOLDS_BELOW_POVERTY
FROM [dbo].[ChicagoCensusData$]
ORDER BY PERCENT_HOUSEHOLDS_BELOW_POVERTY DESC;
'''

# Execute the query and fetch the results
cursor.execute(sql_query)

# Fetch the results
rows = cursor.fetchall()

# Print the table header
print(f'{"COMMUNITY_AREA_NAME":<30}{"Percent_Households_Below_Poverty":<10}')

# Print each row
for row in rows:
    print(f'{row.COMMUNITY_AREA_NAME:<30}{row.PERCENT_HOUSEHOLDS_BELOW_POVERTY:<10}')

```

    COMMUNITY_AREA_NAME           Percent_Households_Below_Poverty
    Riverdale                     56.5      
    Fuller Park                   51.2      
    Englewood                     46.6      
    North Lawndale                43.1      
    East Garfield Park            42.4      
    
8. Which community area is most crime prone? Display the coumminty area number only

```python
# Create a cursor
cursor = connection.cursor()

# SQL query
sql_query = """
SELECT TOP 1 COMMUNITY_AREA_NUMBER, COUNT(COMMUNITY_AREA_NUMBER) AS CRIME_FREQUENCY
FROM [dbo].[ChicagoCrimeData$]
GROUP BY COMMUNITY_AREA_NUMBER
ORDER BY CRIME_FREQUENCY DESC;
"""

cursor.execute(sql_query)

# Fetch the result
result = cursor.fetchone()

# Display the result with column headings
if result:
    column_names = [column[0] for column in cursor.description]
    result_dict = dict(zip(column_names, result))
    print(result_dict)
else:
    print("No results found.")

```

    {'COMMUNITY_AREA_NUMBER': 25.0, 'CRIME_FREQUENCY': 43}
    
9. Use a sub-query to find the name of the community area with highest hardship index


```python

cursor = connection.cursor()


query = '''
        SELECT COMMUNITY_AREA_NAME AS community_area_name
        FROM [dbo].[ChicagoCensusData$]
        WHERE HARDSHIP_INDEX = (SELECT MAX(HARDSHIP_INDEX) FROM [dbo].[ChicagoCensusData$]);
        '''

cursor.execute(query)

# Fetch and print the results with the aliased column name
for row in cursor.fetchall():
    print(row.community_area_name)



```

    Riverdale
    
10.	Use a sub-query to determine the Community Area Name with most number of crimes

```python
cursor = connection.cursor()


query = """
SELECT community_area_name 
FROM [dbo].[ChicagoCensusData$] 
WHERE COMMUNITY_AREA_NUMBER = (
    SELECT TOP 1 COMMUNITY_AREA_NUMBER 
    FROM [dbo].[ChicagoCrimeData$] 
    GROUP BY COMMUNITY_AREA_NUMBER
    ORDER BY COUNT(COMMUNITY_AREA_NUMBER) DESC
)
"""

result = cursor.execute(query).fetchone()

# Print the result
if result:
    print(result.community_area_name)

# Close the cursor and connection when done
cursor.close()
connection.close()


```

    Austin
    
CONCLUSION

Using a Jupyter Notebook to handle a project involving a connection with Microsoft SQL Server Management Studio (SSMS) using the Python library pyodbc is a common and effective practice in the field of data analytics. It allows for the utilization of markdown cells to provide clear and concise documentation. Explain the purpose of the code, steps, and any specific considerations.
Document the connection parameters, such as server name, database name, authentication details, etc.


```python

```


```python

```


```python

```


```python

```
