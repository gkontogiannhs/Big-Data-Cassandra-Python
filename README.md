## Project Aim
This project aims to introduce the reader to technologies used for storing, retrieving and analyzing Big Data, which can come in the form of various data sources.
The focus is on practical data management using open source tools, specifically Apache Cassandra. The retrieval process is done through Python Cassandra API, which allows for easy interaction with the Cassandra database.

## Why Bother
Conventional solutions are often unable to effectively manage such data, but Big Data analysis systems can handle large, heterogeneous and non-uniform data from relational databases, retrieve large data sets from various systems, perform natural language processing, machine learning, artificial intelligence, and prediction based on unstructured data.

## Getting Started
- Data URL: https://www.kaggle.com/grouplens/movielens-20m-dataset
- Install and Cassandra Documentation: https://cassandra.apache.org/doc/latest/cassandra/getting_started/installing.html
  #### OR
- Create your Database in a real cloud-based database service: https://www.datastax.com/products/datastax-astra
- AstraDB & DataStax driver documentation: https://docs.astra.datastax.com/en/landing_page/doc/landing_page/apiDocs.html
- CQL reference: https://docs.datastax.com/en/cql-oss/3.3/cql/cqlIntro.html

## Cassandra Data Modelling 
- Introduction to Cassandra Data Modelling: https://cassandra.apache.org/doc/latest/cassandra/data_modeling/intro.html
- A. Chebotko, A. Kashlev and S. Lu, "A Big Data Modeling Methodology for Apache Cassandra," 2015 IEEE International Congress on Big Data, 2015, pp. 238-245, doi: 10.1109/BigDataCongress.2015.41.

When building a NoSQL database, it is important to start by considering the potential queries that a user might make. From there, the database tables can be constructed in a way that allows for efficient retrieval of relevant data. This approach differs from traditional relational databases, which require tables to be designed based on the schema and relationships between entities. In NoSQL databases, the focus is on optimizing the data for specific use cases and query patterns, rather than adhering to a pre-defined schema. 

**Let's now pretend that that the database we want to build supports a user-interface search and selection of movies to watch (e.g. like Netflix).**
### Possible Queries
1) Show the movies that are popular (have a good rating) within a period of time (e.g. the last 2 months) – this may constitute well the initial "suggestions" screen  to the user
2) Search for the movie(s) that contain some keywords in the title
3) To search movies based on category (genre) and receive them based on some classification (year or grade point average)
4) To see the detailed information about a movie (category, average rating, top-k tags)
5) To see the top-n movies related to some tag
