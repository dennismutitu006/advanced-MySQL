
Search our Knowledge Base
Search
MySQL Performance: How To Leverage MySQL Database Indexing
Posted on October 23, 2020 by Justin Palmer | Updated: March 5, 2021
Category: Series | Tags: Create, Data Management, Database, how to, index, indexing, MariaDB, MySQL, optimization, Performance, Queries, Query, Show Databases
MySQL Performance
MySQL Performance: Identifying Long Queries
MySQL Performance: MyISAM vs InnoDB
MySQL Performance: How To Leverage MySQL Database Indexing
MySQL Performance: MySQL vs. MariaDB
MySQL Performance: Converting MySQL to MariaDB
MySQL Performance: System Config & Routine Maintenance
MySQL Performance: InnoDB Buffers & Directives
MySQL Performance: MyISAM
MySQL Performance: MySQL/MariaDB Indexes
MySQL Performance: Intro to JOINs in SQL
Reading Time: 6 minutes
A Mysql Indexing Logo
Throughout this tutorial, we will cover some of the fundamentals of indexing. As part of the MySQL series, we will introduce capabilities of MySQL indexing and the role it plays in optimizing database performance. Liquid Web recommends consulting with a DBA before making any changes to your production level application.


What is Indexing?
Indexing is a powerful structure in MySQL which can be leveraged to get the fastest response times from common queries. MySQL queries achieve efficiency by generating a smaller table, called an index, from a specified column or set of columns. These columns, called a key, can be used to enforce uniqueness. Below is a simple visualization of an example index using two columns as a key.

+------+----------+----------+
| ROW | COLUMN_1 | COLUMN_2 |
+------+----------+----------+
| 1 | data1 | data2 |
+------+----------+----------+
| 2 | data1 | data1 |
+------+----------+----------+
| 3 | data1 | data1 |
+------+----------+----------+
| 4 | data1 | data1 |
+------+----------+----------+
| 5 | data1 | data1 |
+------+----------+----------+

This analogy compares MySQL indexing to indexing in the back of a book.
Queries utilize indexes to identify and retrieve the targeted data, even if they are a combination of keys. Without an index, running that same query results in an inspection of every row for the needed data. Indexing produces a shortcut, with much faster query times on expansive tables. A textbooks analogy may provide another common way to visualize how indexes function.

When to Enable Indexing?
Indexing is only advantageous for huge tables with regularly accessed information. For instance, to continue with our textbook analogy, it makes little sense to index a children’s storybook with only a dozen pages. It's more efficient to simply read the book to find each occurrence of the word “turtle” than it would be to set up and maintain indexes, query for those indexes, and then review each page provided. In the computing world, those extra tasks surrounding indexing represent wasted resources which would be better purposed by not indexing.

Without indexes, when tables grow to enormous proportions, response times suffer from queries targeting those obtuse tables. Inefficient queries manifest into latency within application or website performance. We commonly identify this latency by using the MySQL slow query log feature. You can find more details about using the slow query log feature in the first article in this series: MySQL Performance: Identifying Long Queries.
Once a colossal table hits its tipping point, it reaches the potential for downtime for applications and websites. Conducting routine evaluations for growing database establishes optimal database performance and sidesteps long queries' inherent interruptions.

MySQL Indexing Pros vs. Cons
There are benefits and downsides to using MySQL indexing, and we'll discuss the significant pros and cons for your consideration. These aspects will guide you to decide whether indexing is an appropriate choice for your situation.

quick data transmissions and ideal for OLAP.
What Information Does One Index?
Selecting what to index is probably the most challenging part to indexing your databases. Determining what is important enough to index and what is benign enough to not index. Generally speaking, indexing works best on those columns that are the subject of the WHERE clauses in your commonly executed queries. Consider the following simplified table:

ID, TITLE, LAST_NAME, FIRST_NAME, MAIDEN_NAME, DOB, GENDER, AGE, DESCRIPTION, HISTORY, ETC...

If your queries rely on testing the WHERE clause using LAST_NAME and FIRST_NAME then indexing by these two columns would significantly increase query response times. Alternately, if your queries rely on a simple ID lookup, indexing by ID would be the better choice.

These examples are merely a rudimentary example, and there are several types of indexing structures built-in to MySQL. The following MySQL page discusses these types of indexes in greater detail, and a recommended read for anyone considering indexing: How MySQL Uses Indexes

What is a Unique Index?

Another point for consideration when evaluating which columns to serve as the key in your index is whether to use the UNIQUE constraint. Setting the UNIQUE constraint will enforce uniqueness based on the configured indexing key. As with any key, this can be a single column or a concatenation of multiple columns. The function of this constraint ensures that there are no duplicate entries in the table based on the configured key.

UNIQUE constraints increase write speeds, a taxation of implementation.
What is a Primary Key Index?

As commonly invoked as the UNIQUE constraint the PRIMARY KEY is used to optimize indexes. This constraint ensures that the designated PRIMARY KEY cannot be of a null value. As a result, a performance boost occurs when running on an InnoDB storage engine for the table in question. This boost is due to how InnoDB physically stores data, placing null valued rows in the key out of contiguous sequence with rows that have values. Enabling this constraint ensures the rows of the table are kept in contiguous order for quicker responses.

Primary Key Index is absolutely necessary for large tables.
Managing Indexes
Keywords for Managing Indexes: dbName, tableName, indexName
Now we will cover some of the basics of manipulating indexes using MySQL syntax. In examples, we will include the creation, deletion, and listing of indexes. Keep in mind, these examples have placeholder entries for the specific keywords. These keywords are self-explanatory by nature for easy reading, and below is an outline of them.

Instead of tableName you can use dbName.tableName.
Listing/Showing Indexes
Tables can have multiple indexes. Managing indexes will inevitably require being able to list the existing indexes on a table. The syntax for viewing an index is below.

SHOW INDEX FROM tableName;

SHOW INDEX FROM tableName; shows all indexes.
Indexing are present on 3 different columns.
Creating Indexes
Index creation has a simple syntax. The difficulty is in determining what columns need indexing and whether enforcing uniqueness is necessary. Below we will illustrate how to create indexes with and without a PRIMARY KEY and UNIQUE constraints.

As previously mentioned, tables can have multiple indexes. Multiple indexing is useful for creating indexes attuned to the queries required by your application or website. The default settings allow for up to 16 indexes per table, increase this number but is generally more than is necessary. Indexes can be created during a table’s creation or added on to the table as additional indexes later on. We will go over both methods below.

Creating too many indexes can add latency, but if you must then increase buffers in MySQL config.
Example: Create a Table with a Standard Index

You can create an index for several columns, using ID as the index.
CREATE TABLE tableName (
ID int,
LName varchar(255),
FName varchar(255),
DOB varchar(255),
LOC varchar(255),
INDEX ( ID )
);

Example: Create a Table with Unique Index & Primary Key

You can create an Primary Key and UNIQUE constraint over several columns.
CREATE TABLE tableName (
ID int,
LName varchar(255),
FName varchar(255),
DOB varchar(255),
LOC varchar(255),
PRIMARY KEY (ID),
UNIQUE INDEX ( ID )
);

Example: Add an Index to Existing Table

CREATE INDEX statement creates an index and names it.
CREATE INDEX indexName ON tableName (ID, LName, FName, LOC);

Example: Add an Index to Existing Table with Primary Key

the CREATE UNIQUE command can add an index to a table ensuring no duplicate data.
CREATE UNIQUE INDEX indexName ON tableName (ID, LName, FName, LOC);

Deleting Indexes
While managing indexes, you may find it necessary to remove some. Deleting indexes is also a very simple process, see the example below:

The DROP INDEX command lets us drop indexes on particular column.
DROP INDEX indexName ON tableName;

Everything you need to know about High Availability
There are many ways to optimize your database for true efficiency. If you would like to learn more or convert the search engines types available in MySQL read through our MyISAM vs. InnoDB tutorial.  Or if you are need of high functioning databases check out our MySQL product page to view different options.


Series Navigation
<< Previous ArticleNext Article >>
Related Articles:
Using MySQL Command Line to Create a User
How to Use Disk Quotas in Dedicated Linux Servers With cPanel
How to Use Disk Quotas in Dedicated Linux Servers with Plesk
Remove a MySQL User on Linux via Command Line
Remove Permissions for a MySQL User on Linux via Command Line
Grant Permissions to a MySQL User on Linux via Command Line
Avatar for Justin Palmer
About the Author: Justin Palmer
Justin Palmer is a professional application developer with Liquid Web

Refer a Friend
Join our mailing list to receive news, tips, strategies, and inspiration you need to grow your business

Your Email Address
Get 33% off the first 3 months on a new VPS!
Categories

Billing + Manage
42

Common Fixes
83

Featured Articles
56

Hosting Basics
155

Other
77

Products
40

Security
27

Series
57

Technical Support
387

Tutorials
896
Have Some Questions?
Our Sales and Support teams are available 24 hours by phone or e-mail to assist.

1.800.580.4985
1.517.322.0434
Latest Articles
TUTORIALS
How to install AWS CLI on Linux (AlmaLinux)

Read Article
SECURITY
Subdomain takeover — protect your website against it!

Read Article
HOSTING BASICS
Controlling PHP settings with a custom php.ini file

Read Article
TECHNICAL SUPPORT
Linux dos2unix command syntax — removing hidden Windows characters from files

Read Article
COMMON FIXES
Change cPanel password from WebHost Manager (WHM)

Read Article
Want More Great Content Sent to Your Inbox?
Your Email Address
CHAT WITH A HUMAN



