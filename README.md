# 🎬 CineRate — Movie Rating & Review Database

CineRate is a relational MySQL database designed for a movie rating and review platform. It manages movies, directors, actors, genres, users, ratings, reviews, and watchlists through a structured relational schema.

The project focuses on database design, relationships, normalization, constraints, and SQL querying.

---

## 📌 Project Overview

CineRate models the core data and interactions of a movie platform.

Users can interact with movies by:

- ⭐ Rating movies (1–10 scale)
- 📝 Writing reviews
- 📋 Managing watchlists

Movies are connected to directors, actors, and genres through relational structures, including many-to-many relationships.

---

## 🗂️ Database Structure

The database contains **10 relational tables**:

| Table | Description |
|---|---|
| `Directors` | Stores director information |
| `Actors` | Stores actor information |
| `Genres` | Stores movie genres |
| `Movies` | Stores movie information, linked to a director |
| `Users` | Stores platform users |
| `Ratings` | Stores user ratings (1–10) for movies |
| `Reviews` | Stores user-written reviews of movies |
| `Watchlist` | Stores movies saved by users |
| `MovieGenres` | Junction table connecting movies and genres |
| `MovieActors` | Junction table connecting movies and actors |

### Relationships

The database contains:

- **7 one-to-many relationships**
- **2 many-to-many relationships**
- **10 primary keys** (one per table)
- **3 CHECK constraints**
- **5 UNIQUE constraints**
- **4 DEFAULT constraints**

Many-to-many relationships are handled using junction tables:

```text
Movies ───< MovieGenres >─── Genres
Movies ───< MovieActors >─── Actors
```

---

## 🔗 Database Relationships

### One-to-Many

| Parent | Child |
|---|---|
| Directors | Movies |
| Users | Ratings |
| Users | Reviews |
| Users | Watchlist |
| Movies | Ratings |
| Movies | Reviews |
| Movies | Watchlist |

### Many-to-Many (resolved via junction tables)

- Movies ↔ Genres (via `MovieGenres`)
- Movies ↔ Actors (via `MovieActors`)

This avoids duplicated data and keeps the database structure organized.

---

## 🔒 Constraints Reference

| Type | Location | Purpose |
|---|---|---|
| CHECK | `Movies.release_year` | Keeps release years within a realistic range (1888–2100) |
| CHECK | `Movies.duration_minutes` | Ensures runtime is a positive number |
| CHECK | `Ratings.rating` | Restricts ratings to a 1–10 scale |
| UNIQUE | `Genres.genre_name` | Prevents duplicate genre names |
| UNIQUE | `Users.username` | Enforces unique usernames |
| UNIQUE | `Users.email` | Enforces unique emails |
| UNIQUE | `Ratings (user_id, movie_id)` | One rating per user per movie |
| UNIQUE | `Watchlist (user_id, movie_id)` | Prevents duplicate watchlist entries |
| DEFAULT | `Users.join_date` | Defaults to the current date |
| DEFAULT | `Ratings.rating_date` | Defaults to the current timestamp |
| DEFAULT | `Reviews.review_date` | Defaults to the current timestamp |
| DEFAULT | `Watchlist.added_date` | Defaults to the current timestamp |

---

## 🧩 Main Features

### 🎥 Movie Management
- Store movie details (title, release year, duration, plot summary)
- Connect movies with directors
- Associate movies with multiple genres
- Associate movies with multiple actors (and their role names)

### 👤 User Management
- Store user account information
- Track user activity (ratings, reviews)
- Manage user watchlists

### ⭐ Ratings & Reviews
- Users can rate movies on a 1–10 scale
- Users can write reviews
- Ratings and reviews are connected to both users and movies

### 📋 Watchlist
- Users can save movies to watch later
- Movies can appear on multiple users' watchlists

---

## 🛠️ Technologies

- MySQL 8.0+
- SQL
- Relational Database Design
- Database Normalization

---

## 🔍 SQL Concepts Used

- `CREATE TABLE`, `INSERT`, `SELECT`, `UPDATE`, `DELETE`
- `JOIN` (inner, left)
- `GROUP BY` / `HAVING`
- Aggregate functions (`AVG`, `COUNT`)
- Subqueries (scalar, correlated, derived tables)
- `WHERE`, `LIKE`, `BETWEEN`
- `GROUP_CONCAT`
- Primary Keys / Foreign Keys
- `CHECK`, `UNIQUE`, `DEFAULT`
- `AUTO_INCREMENT` (identity columns)

---

## 📐 Database Normalization

The database was designed using normalization principles. The tables satisfy:

- **1NF** — First Normal Form
- **2NF** — Second Normal Form
- **3NF** — Third Normal Form

Normalization helps reduce data redundancy and maintain data integrity. The junction tables `MovieGenres` and `MovieActors` correctly represent the many-to-many relationships without duplicating movie, genre, or actor data.

---

## 📊 Example Queries

The project includes SQL queries demonstrating both basic and advanced querying.

**Basic**
- Filtering movies by release year
- Pattern matching movie titles with `LIKE`
- Range filtering runtimes with `BETWEEN`

**Advanced**
- Average rating per movie:

```sql
SELECT
    m.title,
    ROUND(AVG(r.rating), 2) AS average_rating,
    COUNT(r.rating_id)      AS total_ratings
FROM Movies m
JOIN Ratings r ON m.movie_id = r.movie_id
GROUP BY m.movie_id, m.title
ORDER BY average_rating DESC;
```

- Full cast of each movie, joined across the `MovieActors` junction table
- Highest-rated movie per genre, using a derived-table subquery
- Users who rate more actively than the platform average, using a correlated `HAVING` subquery
- Movies on a user's watchlist that they haven't rated yet, using `NOT IN`

See `CineRate_Database.sql` for the full set of 11 example queries, including `UPDATE` and `DELETE` statements.

---

## 📁 Project Structure

```
CineRate/
│
├── CineRate_Database.sql   -- schema, constraints, sample data, example queries
└── README.md
```

---

## 🎯 Learning Objectives

This project was created to practice and demonstrate:

- Relational database design
- Entity relationships
- Primary and foreign keys
- Many-to-many relationships
- Database normalization
- Data integrity constraints (`CHECK`, `UNIQUE`, `DEFAULT`)
- SQL querying, complex joins, aggregate functions, and subqueries
- Database organization

---

## 🚀 Future Improvements

Possible future improvements include:

- Building a frontend interface
- Connecting the database to a backend application
- Adding user authentication
- Implementing movie search and filtering
- Adding movie recommendations
- Adding an API for movie data
- Creating an admin dashboard

---

## 👩‍💻 Author

**Shimaa Nashat**
Front-End Developer & Computer Science Student
