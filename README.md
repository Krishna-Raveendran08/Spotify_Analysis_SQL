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
'''
