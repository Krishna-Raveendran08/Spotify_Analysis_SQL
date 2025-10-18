# 🎵 Advanced SQL Project – Spotify Dataset Analysis

## **Overview**
This project demonstrates advanced SQL skills using a **Spotify dataset**. It focuses on **data cleaning, transformation, aggregation, and analysis** to extract actionable insights about tracks, artists, and albums.

---

## **Tools & Technologies**
- SQL / PostgreSQL / MySQL  
- Data Analysis: Aggregation, Joins, Subqueries, Window Functions  
- Query Optimization: Indexing, EXPLAIN ANALYZE  

---

## **Dataset**
The dataset contains Spotify tracks with fields such as:  
- `artist`, `track`, `album`, `album_type`  
- Audio features: `danceability`, `energy`, `loudness`, `valence`, etc.  
- Engagement metrics: `views`, `likes`, `comments`, `stream`  
- Video info: `official_video`, `licensed`, `channel`  
- Derived metrics: `energy_liveness`, `most_played_on`  

---

## **SQL Queries**

### **Q1: Retrieve the names of all tracks that have more than 1 billion streams**
```sql
SELECT track 
FROM spotify
WHERE stream > 1000000000;
```

**Q2: List all albums along with their respective artists**
```sql
SELECT DISTINCT album, artist 
FROM spotify
ORDER BY 1;
```

**Q3: Get the total number of comments for tracks where licensed = TRUE**
```sql
SELECT SUM(comments) AS total_comments 
FROM spotify
WHERE licensed = 'true';
```

**Q4: Find all tracks that belong to the album type 'single'**
```sql
SELECT track 
FROM spotify
WHERE album_type = 'single';
```

**Q5: Count the total number of tracks by each artist**
```sql
SELECT artist, COUNT(*) AS total_no_songs
FROM spotify
GROUP BY 1
ORDER BY 2;
```

**Q6: Calculate the average danceability of tracks in each album**
```sql
SELECT album, AVG(danceability) AS avg_danceability
FROM spotify
GROUP BY 1
ORDER BY 2 DESC;
```

**Q7: Find the top 5 tracks with the highest energy values**
```sql
SELECT track, MAX(energy) AS max_energy
FROM spotify
GROUP BY 1
ORDER BY 2
LIMIT 5;
```

**Q8: List all tracks along with their views and likes where official_video = TRUE**
```sql
SELECT track, SUM(views) AS total_views, SUM(likes) AS total_likes
FROM spotify
WHERE official_video = 'true'
GROUP BY 1;
```

**Q9: For each album, calculate the total views of all associated tracks**
```sql
SELECT album, track, SUM(views) AS total_views
FROM spotify
GROUP BY 1, 2
ORDER BY 3 DESC;
```
**Q10. Retrieve the track names that have been streamed on Spotify more than YouTube.**
```sql
SELECT * FROM spotify

SELECT * FROM 
(SELECT track,
	COALESCE(SUM(CASE WHEN most_played_on = 'Youtube' THEN stream END),0) as streamed_on_youtube,
	COALESCE(SUM(CASE WHEN most_played_on = 'Spotify' THEN stream END),0) as streamed_on_spotify
FROM spotify
GROUP BY 1)as t1
WHERE 
	streamed_on_spotify > streamed_on_youtube
	AND
	streamed_on_youtube <> 0
```
**Q11. Find the top 3 most_viewed tracks for each artist using window functions.**
```sql
SELECT * FROM spotify

WITH t2 AS
(
SELECT artist, 
	track,
	SUM(views) as total_views,
	DENSE_RANK()OVER(PARTITION BY artist ORDER BY SUM(views) desc) AS rank
FROM spotify
GROUP BY 1,2
ORDER BY 1, 3 DESC
)
SELECT * FROM t2
WHERE rank <= 3;
```

**Q12. Write a query to find tracks where the liveness score is above the average.**
```sql
SELECT track, liveness
FROM spotify
WHERE
liveness > (SELECT AVG(liveness) FROM spotify)
```

**Q13. Find tracks where the energy-to-liveness ratio is greater than 1.2.**
```sql
SELECT * FROM spotify;

SELECT 
    track,
    artist,
    energy,
    liveness,
    (energy / liveness) AS energy_liveness_ratio
FROM spotify
WHERE (energy / liveness) > 1.2;
```
**Q14. Calculate the cumulative sum of likes for tracks ordered by the number of views, using window functions.**
```sql
SELECT 
    track,
    views,
    likes,
    SUM(likes) OVER (
        ORDER BY views DESC
    ) AS cumulative_likes
FROM spotify;
```

**Q15.Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.**

```sql
WITH cte
AS
(SELECT album,
	MAX(energy) as highest_energy,
	MIN(energy) as lowest_energy
FROM spotify
GROUP BY 1
)
SELECT 
	album,
	highest_energy - lowest_energy as energy_diff
FROM cte
ORDER BY 2 DESC
```
**Query Optimization**
```sql
EXPLAIN ANALYZE --PT :0.218 ms, ET : 21.605 ms
SELECT 
	artist,
	track,
	views
FROM spotify
WHERE artist = 'Gorillaz'
	AND
	most_played_on = 'Youtube'
ORDER BY stream DESC LIMIT 25
```
PT :0.218 ms, ET : 21.605 ms before creating the index(query optimization)

**Index Creation**
```sql
CREATE INDEX artist_index ON spotify(artist);
```

after creating the index. The same Query will executed at ET : 1.913 ms.

```sql
EXPLAIN ANALYZE 
SELECT 
	artist,
	track,
	views
FROM spotify
WHERE artist = 'Gorillaz'
	AND
	most_played_on = 'Youtube'
ORDER BY stream DESC LIMIT 25
```





