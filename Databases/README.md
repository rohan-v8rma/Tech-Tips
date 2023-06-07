# INDEX 

- [INDEX](#index)
- [Vertical vs. Horizontal Scaling in Databases](#vertical-vs-horizontal-scaling-in-databases)
  - [Vertical Scaling](#vertical-scaling)
  - [Horizontal Scaling](#horizontal-scaling)
- [Types of DBMS](#types-of-dbms)
  - [Relational DBMS (RDBMS)](#relational-dbms-rdbms)
  - [Object-Oriented DBMS (OODBMS)](#object-oriented-dbms-oodbms)
  - [Hierarchical DBMS](#hierarchical-dbms)
  - [Network DBMS](#network-dbms)
  - [Object-Relational DBMS (ORDBMS)](#object-relational-dbms-ordbms)
  - [Key-Value DBMS](#key-value-dbms)
  - [Columnar DBMS](#columnar-dbms)
  - [In-Memory DBMS](#in-memory-dbms)
  - [Time-Series DBMS](#time-series-dbms)
- [Relational Database](#relational-database)
  - [Tables](#tables)
  - [Keys](#keys)
    - [Primary Key (PK)](#primary-key-pk)
    - [Candidate Key](#candidate-key)
    - [Composite Key](#composite-key)
    - [Foreign Key (FK)](#foreign-key-fk)
    - [Super Key](#super-key)
  - [Joins](#joins)
    - [Inner Join](#inner-join)
    - [Equi Join](#equi-join)
    - [Natural Join](#natural-join)
    - [Cross Join (Cartesian Product)](#cross-join-cartesian-product)
    - [Outer Join](#outer-join)
      - [Left Outer Join](#left-outer-join)
      - [Right Outer Join](#right-outer-join)
      - [Full Outer Join](#full-outer-join)
  - [Indexes](#indexes)
  - [Views](#views)
  - [Stored Procedures](#stored-procedures)
  - [Triggers](#triggers)
- [Non-Relational Database](#non-relational-database)
  - [Collections](#collections)
  - [Documents](#documents)
  - [Keys](#keys-1)
  - [Indexes](#indexes-1)
  - [Embedding](#embedding)
  - [Sharding](#sharding)
  - [Capped Collections](#capped-collections)
- [Real-time Databases](#real-time-databases)
  - [Push notification (TODO: Understand in-depth)](#push-notification-todo-understand-in-depth)
    - [Detailed explanation of push notifications](#detailed-explanation-of-push-notifications)
  - [How real-time databases are used to make online multiplayer games](#how-real-time-databases-are-used-to-make-online-multiplayer-games)
    - [Game state](#game-state)
    - [Synchronization](#synchronization)
    - [Player information](#player-information)
    - [Chat and messaging](#chat-and-messaging)
    - [High-score boards](#high-score-boards)
    - [Player positions (Understand and shorten)](#player-positions-understand-and-shorten)
- [Important Concepts](#important-concepts)
  - [Link between SQL Client and SQL Server](#link-between-sql-client-and-sql-server)
  - [Cases where SQL databases are preferred over NoSQL databases](#cases-where-sql-databases-are-preferred-over-nosql-databases)


# Vertical vs. Horizontal Scaling in Databases

In database management, there are two main ways to scale a database: horizontal scaling and vertical scaling.

## Vertical Scaling

Vertical scaling, also known as scaling up, involves increasing the resources of the existing database server, such as adding more memory, CPU, or storage. This allows the database to handle more load and increase its performance. However, there are limits to how much a single server can be scaled up before it becomes too expensive or reaches a physical limit.

## Horizontal Scaling

Horizontal scaling, also known as scaling out, involves adding more servers to the database system, so that the load is distributed across multiple servers. This allows the database to handle more load and increase its performance. Horizontal scaling is more flexible and cost-effective than vertical scaling, as it allows for incremental increases in capacity without reaching physical limits.

---

When it comes to vertical scaling, it is usually more expensive and has limits, it can be harder to manage and monitor. On the other hand, horizontal scaling is more cost-effective, easier to manage, and allows for incremental increases in capacity, but it requires a distributed database system and more complex configurations.

> ***Note***: Some databases are better suited for vertical scaling while others are better suited for horizontal scaling. 
> 
> For example, `NoSQL` databases like `MongoDB` and `Cassandra` are designed for horizontal scaling, while `SQL` databases like `MySQL` and `PostgreSQL` are more suitable for vertical scaling.

In general, the best approach for scaling a database depends on the specific use case, the amount of data, the number of users and the budget. A combination of both methods can be used to achieve the desired level of performance and scalability.

# Types of DBMS

There are several types of DBMS (Database Management Systems) based on their data model, architecture, and usage. Here are some common types of DBMS:

## Relational DBMS (RDBMS)

Relational DBMS is based on the relational data model. It organizes data into tables with rows and columns, and it establishes relationships between tables using keys. Examples include Oracle, MySQL, Microsoft SQL Server, PostgreSQL, and SQLite.

## Object-Oriented DBMS (OODBMS)

Object-Oriented DBMS stores data in the form of objects, which encapsulate both data and behavior. It supports inheritance, polymorphism, and other object-oriented concepts. Examples include MongoDB, Apache Cassandra, and db4o.

## Hierarchical DBMS

Hierarchical DBMS organizes data in a tree-like structure, where each record has a parent-child relationship. It is suitable for representing hierarchical data, such as file systems. Examples include IBM's Information Management System (IMS) and Windows Registry.

## Network DBMS

Network DBMS is similar to hierarchical DBMS but allows for more complex relationships between records. It uses a network data model, where records are connected in a graph-like structure. Examples include Integrated Data Store (IDS) and Integrated Database Management System (IDMS).

## Object-Relational DBMS (ORDBMS)

Object-Relational DBMS combines the features of relational and object-oriented databases. It extends the relational model to support object-oriented concepts, such as user-defined types, inheritance, and methods. Examples include Oracle, PostgreSQL, and IBM DB2.

## Key-Value DBMS

Key-Value DBMS stores data as a collection of key-value pairs, where each value is associated with a unique key. It provides simple storage and retrieval mechanisms and is often used for caching, session management, and high-speed data access. Examples include Redis, Amazon DynamoDB, and Riak.

## Columnar DBMS

Columnar DBMS stores data in columns rather than rows, which allows for efficient compression, indexing, and analytics on large datasets. It is commonly used for data warehousing and analytical processing. Examples include Apache Cassandra, Vertica, and Google BigQuery.

## In-Memory DBMS

In-Memory DBMS stores data primarily in memory rather than on disk, which provides faster data access and processing. It is designed for high-performance applications and real-time analytics. Examples include SAP HANA, VoltDB, and MemSQL.

## Time-Series DBMS

Time-Series DBMS specializes in storing and analyzing time-series data, such as sensor data, log files, and financial market data. It offers optimized storage and query capabilities for time-ordered data. Examples include InfluxDB, TimescaleDB, and Prometheus.

These are just some of the common types of DBMS. Each type has its own strengths, weaknesses, and specific use cases, and organizations choose the appropriate type based on their data requirements and application needs.

---

# Relational Database

A SQL (Structured Query Language) database is a type of relational database that uses a structured approach to store and manage data. It is organized into a set of tables, each table consisting of rows (records) and columns (fields).

The structure of a SQL database typically includes the following components:

## Tables

The main component of a SQL database, tables are used to store data in a structured format. Each table is composed of rows (records) and columns (fields). Each row represents a unique instance of data, and each column represents a specific piece of data for that instance.

## Keys

### Primary Key (PK) 

A primary key is a unique identifier for each record in a table. It ensures that each record has a unique value for the primary key attribute. Primary keys are used to enforce entity integrity and provide a means to uniquely identify records. In most cases, a primary key is composed of one or more attributes.

### Candidate Key 

A candidate key is a set of attributes that can uniquely identify records in a table. It means that no two records can have the same values for all the attributes in the candidate key. A relation can have multiple candidate keys, but one of them is selected as the primary key.

### Composite Key 

A composite key is a primary key that consists of two or more attributes. It is used when a single attribute is not sufficient to uniquely identify records. The combination of multiple attributes in the composite key ensures uniqueness.

### Foreign Key (FK) 

A foreign key is an attribute or a set of attributes in one table that refers to the primary key in another table. It establishes a relationship between two tables, called a referential integrity constraint. The foreign key in one table references the primary key of another table, creating a link between them.

### Super Key 

A super key is a set of attributes that can uniquely identify records in a table. It may contain more attributes than required to form a candidate key. In other words, a super key is a superset of a candidate key. It can have additional attributes that are not necessary for uniqueness.

## Joins

### Inner Join

An inner join returns only the matching rows from both tables involved in the join. It combines rows from two tables based on a related column between them, known as the join condition. The result includes only the rows where the join condition is satisfied.

### Equi Join

An equi join is a specific type of inner join that uses the equality operator (=) to match rows between two tables based on a common column. It retrieves the rows where the values in the join columns of both tables are equal.

### Natural Join

A natural join is a type of inner join that matches rows between two tables based on columns with the same name and compatible data types. It automatically determines the join condition by looking for the common column names.

### Cross Join (Cartesian Product)

A cross join, also known as a Cartesian product, combines all rows from one table with all rows from another table. It results in a combination of every row from the first table with every row from the second table, resulting in a total number of rows equal to the product of the row counts in both tables.

### Outer Join

#### Left Outer Join

A left outer join returns all rows from the left table (the table mentioned before the LEFT OUTER JOIN keyword) and the matching rows from the right table. If there is no match in the right table, NULL values are included for the right table columns.

#### Right Outer Join

A right outer join returns all rows from the right table (the table mentioned before the RIGHT OUTER JOIN keyword) and the matching rows from the left table. If there is no match in the left table, NULL values are included for the left table columns.

> ***Note***: Suppose we have two tables, `A` and `B`. 
> 
> `A LEFT OUTER JOIN B` and `B RIGHT OUTER JOIN A` will yield us the same result.

#### Full Outer Join

A full outer join returns all rows from both tables, matching rows from each table, and including NULL values for non-matching rows on both sides. It combines the results of both the left outer join and the right outer join.

---

## Indexes 

Indexes are used to improve the performance of data retrieval in a SQL database. They are used to quickly locate specific rows in a table based on the values in a specific column. Indexes can be created on one or multiple columns and can be unique or non-unique.

## Views 

A view is a virtual table that is based on the result of a SELECT statement. It does not store data but rather presents data from one or more tables in a specific format. Views can be used to simplify data access, hide sensitive data, or present data in a specific format for a specific user or application.

## Stored Procedures 

Stored procedures are pre-compiled sets of SQL statements that can be executed by the database management system. They can be used to perform complex operations on the data, such as data validation, data manipulation, and data retrieval.

## Triggers 

Triggers are special types of stored procedures that are automatically executed by the database management system in response to specific events, such as a data insert, update, or delete. They are used to automatically maintain data integrity, enforce business rules, and audit changes to the data.

---

All these components work together to provide a robust and efficient way to store, query, and manage data in a SQL database.

# Non-Relational Database

A non-relational or NoSQL database is a type of database that does not use the traditional table-based structure of a relational database. Instead, it uses a more flexible and scalable data model that is better suited for handling large amounts of unstructured or semi-structured data.

The structure of a NoSQL database typically includes the following components:

---

## Collections 

The main component of a NoSQL database, collections are used to store data in a flexible format. Collections are similar to tables in a relational database but unlike relational tables, collections are not required to have a fixed schema. This means that data can be added to a collection without the need to define a table structure in advance.

## Documents 

A document is a single instance of data in a NoSQL database. Unlike rows in a relational database, documents can have different fields and structures. Documents are similar to JSON or XML data and they can be nested.

## Keys 

A key is a unique identifier for each document in a collection. It is used to retrieve and update a specific document.

## Indexes 

Indexes are used to improve the performance of data retrieval in a NoSQL database. They can be created on one or multiple fields and can be used to quickly locate specific documents based on the values in those fields.

## Embedding 

Embedding is a technique used to store related data within a single document. This allows for efficient querying and retrieval of data, as related data is stored in the same document.

## Sharding 

Sharding is a technique used to distribute data across multiple servers. This allows for horizontal scaling, which can improve performance and increase the capacity of a NoSQL database.

## Capped Collections 

Capped collections are collections that have a fixed size. This is useful when you want to keep a fixed amount of data but still want to keep the most recent data.

---

Each of these components work together to provide a flexible and scalable way to store and manage data in a NoSQL database. The structure of a NoSQL database is highly dependent on the specific type of NoSQL database, but generally, they are schema-less and highly scalable.

# Real-time Databases

A real-time database is a type of database that allows for real-time data processing and updates. 

This means that *any* changes made to the data in the database are immediately reflected in all connected clients, applications, or systems.

In a real-time database, data is typically stored in a NoSQL format, which allows for faster and more flexible data storage and retrieval. This is in contrast to traditional relational databases which uses SQL to interact with the database, that are optimized for transactional processing and may take longer to update.

Examples of real-time databases include Firebase and RethinkDB, these are used to store and manage data in a variety of use cases such as online gaming, chat applications, IoT, and more. These databases are designed to handle large amounts of concurrent users and updates, and they can be easily integrated with web and mobile applications.

## Push notification (TODO: Understand in-depth)

Real-time databases use push notifications to update connected clients in real-time, this is done by the use of [web sockets](../Networking/README.md#web-socket), which are a bidirectional communication channel between a client and a server. 

This allows for a low-latency and high-throughput data transfer, and it makes it possible for clients to receive updates as soon as they occur on the server.

### Detailed explanation of push notifications

When a change is made to the data in the database, the server sends a push notification to all connected clients, letting them know that the data has been updated. The clients then retrieve the updated data from the server.

Push notifications are particularly useful in real-time applications, such as chat apps, online gaming, and collaborative tools, where data updates need to be reflected in real-time across all connected clients.

For example, in a chat application, when a user sends a new message, the server sends a push notification to all connected clients, and the message is immediately displayed on the chat screen of all clients.

Push notifications are also used in IoT to notify clients of changes in the status of devices, such as when a sensor value changes or when a device goes offline.

> ***Note***: Push notifications are not always a feature of all real-time databases, but it's a common feature in many modern real-time databases such as Firebase, RethinkDB, and others.

## How real-time databases are used to make online multiplayer games

Real-time databases are used to make online games by providing a way to store and manage game state and player information in real-time. They allow for fast and efficient communication between the game server and the clients, which is critical for providing a smooth and responsive gaming experience.

The real-time databases are used in several ways to make online games:

---

### Game state

The game state, such as player position, score, inventory, and other game-related information, is stored in the real-time database. This allows for the game server to easily access and update the game state as needed.

### Synchronization

Real-time databases allow for fast and efficient synchronization of game state between the game server and the clients. This ensures that all clients have the most up-to-date information about the game state, which is critical for providing a smooth and responsive gaming experience.

### Player information

Player information, such as account information, high scores, and saved games, is stored in the real-time database. This allows for players to easily access and update their information, and for the game server to easily retrieve and update player information as needed.

### Chat and messaging

Real-time databases can also be used to handle chat and messaging in online games. When a player sends a message, the real-time database can be used to quickly send the message to all connected clients, ensuring that all players receive the message in real-time.

### High-score boards

Real-time databases can also be used to handle leaderboards and high-score boards in online games. They allow for easy updates of scores and rankings, and for quick retrieval of leaderboard data by clients.

### Player positions (Understand and shorten)

In a multiplayer game, it's important for all clients to have accurate information about the positions of other players in order to provide a smooth and responsive gaming experience. Real-time databases can be used to store and manage player positions in real-time.

When a player's position changes, the game client sends an update to the game server which in turn updates the player's position in the real-time database. The server then sends a push notification to all connected clients, letting them know that the player's position has been updated. The clients then retrieve the updated position from the real-time database and update the position of the player in their game.

This real-time synchronization of player positions ensures that all clients have the most up-to-date information about the positions of other players, which is critical for providing a smooth and responsive gaming experience.

Additionally, real-time databases can also be used to store other information related to players such as health, inventory, or other attributes that need to be updated and synced in real-time

---

Real-time databases use push notifications, web sockets, and other technologies to ensure low-latency and high-throughput data transfer between the game server and the clients. This allows for a smooth and responsive gaming experience, and it makes it possible for players to receive updates as soon as they occur on the server.

# Important Concepts

## Link between SQL Client and SQL Server

When we try to access the SQL server from our SQL client, we enter the required login details (IP address and port of the server, along with the password). Now, when we enter commands in our CLI, we are essentially running the queries directly in our SQL server. These commands are parsed by our SQL server, executed on our database, and the output is displayed.

When we try to access the SQL server from python, we use the mysql.connector API, to which we give the query we want to run as an argument. The SQL server then receives the request, executes the query and returns the output. This output can then be printed in python.

## Cases where SQL databases are preferred over NoSQL databases

SQL databases are still widely used and preferred in some applications, despite the growing popularity of NoSQL databases.

One of the main reasons for this is that SQL databases have been around for a long time and have a proven track record of reliability and stability. They are also well understood by many developers and IT professionals, which makes them a familiar and comfortable choice.

SQL databases are also well suited for transactional systems, where data consistency and integrity are critical. They use a relational model and enforce relationships between tables using foreign keys and indexes, this allows for complex and sophisticated queries to be performed. Additionally, SQL databases are well suited for business intelligence and data warehousing, where large amounts of structured data need to be analyzed and reported on.

Another important aspect is that SQL databases often have more advanced security features, such as encryption and access control, which are critical for some applications.

However, it's worth noting that SQL and NoSQL databases are not mutually exclusive, and in many cases, a combination of both can be used to achieve the best results. For example, you could use a NoSQL database for handling high-performance, high-volume data and a SQL database for handling data that requires complex queries and transactions.

In summary, SQL databases are still preferred over NoSQL databases in some applications, particularly where data consistency, integrity, and security are critical, and where large amounts of structured data need to be analyzed and reported on. But as technology and data evolves, NoSQL databases are becoming more and more capable and are starting to be a viable alternative for most use cases.