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
