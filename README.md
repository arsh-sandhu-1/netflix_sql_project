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
## Business Problems and Solutions
### 1. Count the Number of Movies vs TV Shows
```sql
SELECT 
	type , 
	COUNT(*) 
FROM netflix 
GROUP BY 1 ;
```
### 2. Find the most common rating for movies and TV shows

```sql
SELECT 
	type ,
	rating
FROM 
	(
	SELECT 
		type,
		rating,
		COUNT(*),
		RANK() OVER(PARTITION BY type ORDER BY COUNT(*) DESC) as ranking
		FROM netflix
		GROUP BY 1 ,2
		ORDER BY 3 
		) as t1 
	WHERE ranking = 1;
```

 ### 3. List all movies released in a specific year (e.g., 2020)

```sql
SELECT * 
	FROM netflix 
	WHERE type = 'Movie' 
	AND 
	release_year = 2020 ;
```


### 4. Find the top 5 countries with the most content on Netflix
```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(country, ',')),
	COUNT(show_id) as total_content
	FROM netflix 
	GROUP BY 1
	ORDER BY 2 DESC 
	LIMIT 5;
```

### 5. Identify the longest movie
```sql
SELECT * FROM netflix
	WHERE type = 'Movie'
	AND duration = (SELECT MAX(duration) FROM netflix);
```


### 6. Find content added in the last 5 years
```sql
SELECT 
	* 
	FROM netflix 
	WHERE TO_DATE(date_added, 'Month DD,YYYY') >= CURRENT_DATE - INTERVAL '5 years';
```

### 7. Find all the movies/TV shows by director 'Rajiv Chilaka'!
```sql
	SELECT * 
		FROM netflix 
		WHERE director ILIKE '%Rajiv Chilaka%';
```

### 8. List all TV shows with more than 5 seasons
```sql
SELECT * 
	FROM netflix
	WHERE 
	type = 'TV Show'
	AND 
	SPLIT_PART(duration, ' ',1)::numeric > 5;
```

### 9. Count the number of content items in each genre
```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(listed_in, ',')) as genre,
	COUNT(show_id) as total_content
FROM netflix
GROUP BY 1
```

### 10. Find each year and the average numbers of content release by India on netflix. 
### return top 5 year with highest avg content release !
```sql
SELECT 
	EXTRACT(YEAR FROM TO_DATE(date_added, 'Month DD, YYYY')) as year_added,
	COUNT(*) as yearly_content,
	ROUND(
	COUNT(*)::numeric/(SELECT COUNT(*) FROM netflix WHERE country = 'India')::numeric * 100,2) as avg_content_per_year
	FROM netflix
	WHERE contry = 'India';
```

### 11. List all movies that are documentaries
```sql
SELECT * 
	FROM netflix
	WHERE 
	listed_in ILIKE '%documentaries%';
```

### 12. Find all content without a director
```sql
SELECT * 
	FROM netflix
	WHERE director IS NULL;
```

### 13. Find how many movies actor 'Salman Khan' appeared in last 10 years!
```sql
SELECT *
	FROM netflix 
	WHERE casts ILIKE '%Salman Khan%'
	AND 
	release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10;
```

### 14. Find the top 10 actors who have appeared in the highest number of movies produced in India.
```sql
SELECT 
UNNEST(STRING_TO_ARRAY(casts, ',')) as actors,
COUNT(*) as total_content
FROM netflix 
WHERE country ILIKE '%india%'
GROUP BY 1 
ORDER BY 2 DESC 
LIMIT 5 ;
```

###  15.Categorize the content based on the presence of the keywords 'kill' and 'violence' in the description field. Label content containing these keywords as 'Bad' and all other content as 'Good'. Count how many items fall into each category
	
```sql
SELECT 
	category,
	type,
	COUNT(*) AS content_count
	from
	(
	SELECT 
		*,
		CASE
		WHEN description ILIKE '%kill' OR description ILIKE '%violence' THEN 'Bad'
		ELSE 'Good'
		END AS category 
		FROm netflix 
	) AS categorized_content
	GROUP BY 1,2
	ORDER BY 2;
```
## 🔍 Key Findings
1. Content Split: A clear distribution exists between Movies and TV Shows, with Movies forming the majority.

1. Ratings: Certain ratings dominate each format (e.g., TV-MA for shows, TV-14 for movies).

1. Regional Leaders: The USA and India are among the top contributors to Netflix’s library.

1. Longest Movie: The dataset includes unusually long films, indicating diversity in production style.

1. Recent Additions: A significant volume of content has been added in the last 5 years, showing Netflix’s aggressive expansion.

1. Director Insights: Rajiv Chilaka emerges as a notable figure in specific genres.

1. Series Trends: Several TV shows run beyond 5 seasons, appealing to long-term audiences.

1. Actor Spotlight: Frequent appearances by top Indian actors highlight regional influence.

1. Genre Spread: Documentaries and drama dominate certain niches.

1. Content Classification: Titles with violence-related keywords form a small but significant portion of the catalog.

## 📈 Conclusion
Through SQL-driven analysis, we can clearly see how Netflix curates a diverse library, balancing global appeal with targeted regional hits.
The insights help understand content strategy, audience targeting, and the evolving mix of entertainment formats.
This approach can be scaled to monitor trends, identify growth opportunities, and support data-driven decision-making in the OTT space.
