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
