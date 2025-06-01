# Big-Data-Project
## IMDb Data Pipeline & Analysis

### Project Overview
#### Scope
##### a. Ingestion
- Target: Scrape a few hundred movies from IMDb.
- Attributes: Extract 20–30 attributes per movie.
- Volume: A few hundred movies

##### b. Analysis
- Storage: Goggle BigQuery
  
##### c. Visualization
- Tools: Google Looker Studios
- Goal: Build easy-to-use tools for end-users to explore and analyze the data.

#### Tools & Technology
1. Python (pip manager) for scraping.
2. Google BigQuery for Storage and Analysis.
3. Pandas for Pipeline/ Data Handling.
4. Google Looker Studio for Visualization.

#### 1. Web Scraping IMDb
a. Target Page: 
Please note that I used TMDB instead: https://www.themoviedb.org/


b. Data Points| Category                    | Data Points                                                                                 |
| --------------------------- | ------------------------------------------------------------------------------------------- |
| 🎬 **Core Info**            | `id`, `title`, `original_title`, `release_date`, `runtime`, `status`, `tagline`, `overview` |
| 🌐 **Language/Country**     | `original_language`, `spoken_languages`, `production_countries`                             |
| 💸 **Financials**           | `budget`, `revenue`                                                                         |
| 🌟 **Popularity & Ratings** | `vote_average`, `vote_count`, `popularity`, `adult`                                         |
| 🧑‍💼 **People**            | `director`, `cast`                                                                          |
| 🏢 **Companies**            | `production_companies`                                                                      |
| 🖼️ **Media Assets**        | `backdrop_path`, `poster_path`, `homepage`, `imdb_id`                                       |
| 🧾 **Categorization**       | `genres`                                                                                    |

c. Setup and Run
```bash
pip install pandas
pip install tmdb
python data_scrapping.py
```

#### 2. Analysis Layer
##### Storing in Google BigQuery
Project Link: https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=github_repos&page=dataset&invt=AbynjQ&project=fluted-arcadia-452911-h6&ws=!1m28!1m4!1m3!1sfluted-arcadia-452911-h6!2sbquxjob_61512a32_1972c6c2bc4!3seurope-central2!1m3!8m2!1s990269497283!2sd29693ad7e7346a0b60575a420d67906!1m4!1m3!1sfluted-arcadia-452911-h6!2sbquxjob_60b7e878_1972c8b5191!3seurope-central2!1m4!1m3!1sfluted-arcadia-452911-h6!2sbquxjob_1a7257e4_1972c604d3b!3seurope-central2!1m4!1m3!1sfluted-arcadia-452911-h6!2sbquxjob_7dc51c_1972c673016!3seurope-central2!1m3!3m2!1sfluted-arcadia-452911-h6!2sbig_data_project&inv=1&authuser=1

##### Data Analysis (Sample Queries)
1. Countries Representation Report
  ```sql
  SELECT country, COUNT(*) AS movie_count
  FROM `big_data_project.imdb_dataset`,
       UNNEST(SPLIT(production_countries, ', ')) AS country
  GROUP BY country
  ORDER BY movie_count DESC
  LIMIT 10;
```
2. Movies ROI Report
```sql
  SELECT 
    title,
    budget,
    revenue,
    FORMAT('%.2f', ROUND((revenue - budget) / NULLIF(budget, 0), 2)) AS roi
  FROM `big_data_project.imdb_dataset`
  WHERE budget > 0 AND revenue > 0
  ORDER BY roi DESC
  LIMIT 10;
```
3. Underrated Movies Report
```sql
  SELECT title, vote_average, popularity, vote_count
  FROM `fluted-arcadia-452911-h6.big_data_project.imdb_dataset`
  WHERE vote_average <= 5.0 AND popularity < 300
  ORDER BY vote_average DESC
  LIMIT 20;
```

#### 3. Presentation Layer
##### Interactive Dashboards (Google Looker Studio)
- [Countries Representation Report](https://lookerstudio.google.com/s/kq-Mp3-cPJY)
- [Movies ROI Report](https://lookerstudio.google.com/s/u_FVL1QUZO8)
- [Underrated Movies Report](https://lookerstudio.google.com/s/tlqkT7VdbsQ)


