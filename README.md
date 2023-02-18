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
2) Show the he movie(s) that contain some keywords in the title
3) Show the movies based on category (genre) and receive them based on some classification (year or average rating)
4) Show the detailed information about a movie (category, average rating, top-k tags)
5) Show the top-n movies related to some tag

| Type | Diagram |
| --- | --- |
| Entity Relationship  | <img src="https://github.com/gkontogiannhs/Big-Data-Cassandra-Python/blob/main/snaps/ER.png" width="500" height="400"> |
| Application Workflow | <img src="https://github.com/gkontogiannhs/Big-Data-Cassandra-Python/blob/main/snaps/AW.png" width="500" height="400">  |
| Chebotko | <img src="https://github.com/gkontogiannhs/Big-Data-Cassandra-Python/blob/main/snaps/chebotko.png" width="500" height="400"> |

## DataBase Implementation
The structure of the database will be created by shell statements. The data insertion is done via python scripts [cassandra_insert.ipynb](https://github.com/gkontogiannhs/Big-Data-Cassandra-Python/blob/main/cassandra_insert.ipynb) using dsbulk, after data are modelled (denormalization etc.) based on the queries requirements.

### DDL Statements
```
# Create an empty Cassandra keyspace
CREATE KEYSPACE <big_data>
```

```
# 1st Query DDL
CREATE TABLE big_data.popular_movies_by_date (
    year int,
    month int,
    day int,
    rating float,
    movieid int,
    genres text,
    title text,
    PRIMARY KEY (year, month, day, rating, movieid)
) WITH CLUSTERING ORDER BY (month ASC, day ASC, rating DESC, movieid ASC)
```

```
# 2nd Query DDL
CREATE TABLE big_data.movies_by_keyword (
    keyword text,
    rating float,
    movieid int,
    genres text,
    title text,
    PRIMARY KEY (keyword, rating, movieid)
) WITH CLUSTERING ORDER BY (rating DESC, movieid ASC)
```

```
# 3rd Query DDL
CREATE TABLE big_data.movies_by_genre (
    genre text,
    release_date text,
    avg_rating float,
    movieid int,
    title text,
    PRIMARY KEY (genre, release_date, avg_rating, movieid)
) WITH CLUSTERING ORDER BY (release_date ASC, avg_rating DESC, movieid ASC)
```

```
# 4th Query DDL
CREATE TABLE big_data.movie_info_by_title (
    title text,
    tag_count int,
    avg_rating float,
    movieid int,
    tag text,
    genres text static,
    PRIMARY KEY (title, tag_count, avg_rating, movieid, tag)
) WITH CLUSTERING ORDER BY (tag_count DESC, avg_rating DESC, movieid ASC, tag ASC

```

```
# 5th Query DDL
CREATE TABLE big_data.movies_by_tag (
    tag text,
    avg_rating float,
    movieid int,
    genres text,
    title text,
    PRIMARY KEY (tag, avg_rating, movieid)
) WITH CLUSTERING ORDER BY (avg_rating DESC, movieid ASC)
```
