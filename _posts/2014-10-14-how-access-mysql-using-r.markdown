---
layout:     post
title:      Access MySQL using R
date:       2014-10-14 08:47:19
summary:    R provides an easy way of accessing MySQL database and retrieving database for analysis. 
categories: R Database
---

For data scientists, getting and cleaning data is one of most important step in their data analysis work flow. High quality data paves the way for well structured and executed data analysis, whereas poor quality data can often lead to misguided discoveries.  

Relational DBMS such as MySQL and Oracle are frequently used to store preprocessed and structured data. It is therefore important to understand how to connect to relational databases and retrieve required information from its tables. In this post, I will cover how to use R to access MySQL databases. 

### Setup

Before you can connect to MySQL, you need to install RMySQL packages in your R. If you are using Mac, it is fairly straightforward. Simply type the following command in your R terminal. 

	> install.packages("RMySQL")

If you are using Windows, it is a bit more complicated. But this is already well documented on [RMySQL official site](http://cran.r-project.org/web/packages/RMySQL).

### Connect to MySQL

Now you are ready to go, make sure that you have an existing MySQL database which you have access permission. Or, you can [install MySQL](http://dev.mysql.com/doc/refman/5.1/en/installing.html) locally and create a test database yourself. 

Here, I am using the public facing MySQL instance provided by the Proteomics Identifications data ([PRIDE](http://www.ebi.ac.uk/pride)). It stores the protein and peptide identification evidences produced by mass spectrometers. Here are the database connection details:

	Host: 193.62.194.210
	Port: 5000 
	Database: pride_2

If you want to use the same database as your playground, please check the connection command below for user name and password. But do be careful with the mass selects and joins, as these queries can consume a lot of resources and return many rows of data.

Connect to the database using the following command:
	
	> con <- dbConnect(MySQL(), user="inspector", password="inspector", 
		dbname="pride_2", host="193.62.194.210", port=5000)

### Query MySQL

So you are now connected, you try the following commands to list tables and list the fields in a table:
	
	# list tables
	> dbListTable(con)

	# list fields in the 'pride_experiment' table
	> dbListFields(con, "pride_experiment")	

You can also do a select query:

	# count the number of rows in the 'pride_experiment' table
	> dbGetQuery(con, "select count(*) from pride_experiment")

Or if you want to page your results:

	# get the first 100 rows from the 'pride_experiment' table
	> rs <- dbSendQuery(con, "select * from pride_experiment limit 1000")
	> data <- fetch(rs, 100)

	# or you can get all the records in one go using -1
	> data <- fetch(rs, -1)

	# once you are done, you should clear the result set
	> dbClearResult(rs)


### Close your connections

Finally, it is important that you remember to close the connection once you done:

	> dbDisconnect(con)		


### Other resources

[RMySQL vignette](http://cran.r-project.org/web/packages/RMySQL/RMySQL.pdf) provides a list of commonly used commands

[MySQL and R](http://www.r-bloggers.com/mysql-and-r/) is a nice blog post on additional RMySQL tips. 
