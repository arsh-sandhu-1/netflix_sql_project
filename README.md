# Netflix Movies and TV Shows Data Analysis using SQL 
![Netflix logo](https://github.com/arsh-sandhu-1/netflix_sql_project/blob/main/logo.png)
## 📌 Overview
This project analyzes Netflix’s catalog using SQL to uncover insights about the types of content, release patterns, regional contributions, top actors, and more.
A relational dataset containing movie and TV show metadata (title, director, cast, release year, rating, country, genres, duration, and description) was explored using PostgreSQL queries.
## 🎯 Objective
- Compare content distribution between Movies and TV Shows.

- Identify popular ratings, genres, and contributing countries.

- Analyze release trends by year and geography.

- Detect high-performing actors and directors.

- Categorize content based on description sentiment (e.g., presence of violence-related keywords).
## Dataset 
- Dataset link [Netflix Dataset](https://github.com/arsh-sandhu-1/netflix_sql_project/blob/main/netflix_dataset.csv)
## Schema 
```sql
DROP TABLE IF EXISTS netflix;
CREATE TABLE netflix
(
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);

SELECT * FROM netflix ;
```
