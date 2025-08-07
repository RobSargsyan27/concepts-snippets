# Data Warehousing

## Prerequisites

### Database Types:

Before we start with data warehousing, it is important to differentiate different type of databases.

- **Conventional database**: ****Is a broader term that refers to traditional databases which includes `hierarchical`, `network` and `relational` databases. But nowadays when one says conventional database he usually means relational database as this is the most popular conventional database.
- **Document Databases**: A document database is a type of NoSQL database that `stores data as documents`, usually in JSON, BSON, or XML format. Each document represents a record, like a row in a relational database, but with a flexible structure.
- **Operational database**: Is a database modeling approach aimed to support `intense write operations` in highly concurrent environment. This kind of database is configured to support mostly write operations and provide consistent real-time dataset.
- **Analytical database**: Is a database modeling approach which supports `heavy aggregation operations` over the database. This kind of database is configured to efficiently execute aggregation operations over the database. Is used to get insights (generate reports, implement complex calculations etc.) about the data stored in database tables.

<aside>
📌

The operational and analytical database modeling approaches are basically opposite of each other because one is designed for complex write and another for complex read operations. Thus their explicit design and internal database configuration also differ.

</aside>

Four Data Analytics Types

- **Descriptive**: Answers to “What happened?” question. Summarizes past data and displays historical performance.
- **Diagnostic**: Answers to “Why did it happen?” question. Digs deeper to understand the root causes of trends and anomalies.
- **Predictive**: Answers to “What is likely to happen?” question. Uses historical data and models to forecast future outcomes.
- **Prescriptive**: Answers to “What should we do about it?” question. Recommends actions or decisions to achieve desired outcomes.

## Definition

A data warehouse is the only queryable source of data in the enterprise. It’s based on a frequently updated copy of transactional data, and it’s specifically structured for query and analysis. This is a special sub-type of analytical database. 

It aims to provide insights about data from multiple sources using different aggregation operations, that one can not do directly on regular relational or other type of databases due to the database’s design, configuration or other constraints. Data warehouses are able to work with transactional data which is usually stored in structured or semi-structured way.

## Three Data Warehouse Tiers

The data warehouse consists of three tiers. In bottom tier, where the data from different sources are extracted, transformed and loaded (ETL). It is also possible that the data is extracted, loaded and only then transformed. On this tier, the data is getting centralized and consistent. The middle tier is the online analytical processing tier. On this tier the aggregation is being done on data. There are different OLAP (Online Analytical Processing) approaches which aims to optimize the performance of different analytical queries. The top ****tier is the one that displays data to the end-user.  

## OLAP vs OLTP

| **Feature** | **OLAP (Analytical)** | **OLTP (Transactional)** |
| --- | --- | --- |
| Use case | Analysis, reporting | Day-to-day operations |
| Query type | Complex, read-heavy | Simple, write-heavy |
| Data volume | Large, historical | Current, real-time |
| Normalization | Denormalized (star/snowflake) | Highly normalized |
| Example | “Show average sales per product by quarter” | “Insert a new order” |

`OLAP`  (Online Analytical Processing) is provided by tools like PowerBI, in order to run complex queries on data warehouse’s data. It supports fast and interactive analysis of the data. OLAP tools are specifically designed to run complex queries on big datasets with high performance. 

`OLTP` (Online Transaction Processing) is being supported by most of the modern database management systems, they are specifically designed for transactional, highly-concurrent write-heavy operations. 

<aside>
📌

The OLTP is being done on a regular database which is highly normalized, while OLAP is being done on data warehouses, which are highly denormalized

</aside>

## Star and Snowflake Schemas

Both star and snowflake schemas are principles of designing highly denormalized data model.

The `star`  schema consist of a single `fact`  table and multiple `dimenshion`  tables. The fact table only stores entity-related measurable data as well as primary key and foreign keys of all related dimension tables. Dimension table stores readable content which is related to fact table’s entities (city, status, label, etc.) as well as natural key and surrogate key as a primary key. Natural key is important for dimensions especially in case they are slowly changing dimensions of type two or above (see below).

<aside>
📌

There can be one-to-many relationship between fact table and dimension tables. While fact table usually contains many rows but only a limited number of columns, dimension table contains as few rows as possible but can have many columns.

</aside>

The `snowflake` ****schema is a bit normalized version of star schema. This schema type isn’t widely used. In this schema the dimension tables can also have child tables.

### Steps for Designing Star Schema

1. Define the fact table
    1. Determine the core business process to be analyzed (e.g., sales, transactions, inventory).
    2. Identify measurable metrics (e.g., revenue, quantity sold, cost).
2. Identify dimension tables
    1. Determine the key descriptive attributes needed for the report (e.g., time, product, customer).
    2. Design dimension tables with surrogate keys and meaningful attributes.
    3. Is the dimension table a slowly changing dimension? Mark down which type of SCD you need.
3. Design the star schema
    1. Structure the fact table at the center with foreign keys linking to dimension tables.
    2. Ensure a clear relationship between dimensions and facts.

## Slowly Changing Dimensions (SCD)

The SCDs are approaches of storing historical data in dimension tables. Different types of SCD maintain historical data in different levels.

| Type | History Tracked? | Description | Common Use Case |
| --- | --- | --- | --- |
| 0 | No | Keep original values, ignore changes | Immutable attributes (e.g., birthdate) |
| 1 | No | Overwrite with latest values | Correct typos, no need for history |
| 2 | Full history | Add a new row for each change | Track changes over time (e.g., address) |
| 3 | Limited history | Add new columns to store previous values | Keep only current and one previous value |
| 4 | Full history | Archive changes in a separate history table | Very large or rarely-used history |
| 6 | Hybrid | Combine Types 1, 2, and 3 | Advanced models needing both current & historical views |

<aside>
📌

It is required to have a natural key in dimension table in order to be able to track historical changes of the same entity. In some cases it is considered good practice to add dimension’s natural key in fact table as in order to enhance querying performance. 

</aside>
