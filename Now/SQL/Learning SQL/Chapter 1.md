# Chapter 1. A little background

A database is a set of related information.

- Non-relational database systems, 

  - Hierarchical database system, data is represented as one or more tree structures. *single-parent hierarchy*.
  - Network database system, basically *multi-parent hierarchy*.

- Relational model

  - Data is represented as sets of tables, rather than pointers to navigate between related entities.
  - Terms and definitions
    - `Entity`, Something of interest to the database user community. Examples include customers, parts, geographic locations, etc.
    - `Column`, An individual piece of data stored in a table
    - `Row`, A set of columns that together completely describe an entity or some action on an entity. Also called a record.
    - `Table`, A set of rows, held either in memory or on permanent storage
    - `Result set`, Another name for non-persistent table, generally the result of an SQL query
    - `Primary key`, One or more columns that can be used as a unique identifier for each row in a table
    - `Foreign key`, One or more columns that can be used together to identify a single row in another table

- What is SQL

  - SQL is a non-procedural language,
    - SQL statements define the necessary input and outputs, execution is left to the *optimizer*.
    - *optimizer hints* can be used to influence the optimizer's decision.

  

