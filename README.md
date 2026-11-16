# 🎵 Moosic: Automated Playlist Generation with Unsupervised Machine Learning

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-K--Means-orange?style=flat&logo=scikitlearn)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-150458?style=flat&logo=pandas)
![Status](https://img.shields.io/badge/Status-Prototype-success?style=flat)

## 📌 Project Overview

**Moosic** is a data engineering and unsupervised machine learning prototype designed to automate playlist curation[cite: 2]. Acting as a data engineer for a music streaming company, the goal of this project was to determine whether an algorithm can group **5,235 Spotify tracks** into cohesive, mood- and sound-consistent playlists without human intervention or predefined genre labels[cite: 1, 2, 3, 6].

Because a machine cannot "hear" audio, it relies entirely on numerical audio features extracted from Spotify[cite: 3]. Using **K-Means clustering**, the pipeline groups tracks with similar acoustic profiles to generate ready-to-use playlists[cite: 1, 3].

> **Core Verdict:** The prototype works well as a fast, scalable first step that captures acoustic "vibes" (loud vs. quiet, acoustic vs. electric), though human curation and richer metadata (genre, language, era) are needed for a production-ready product[cite: 2, 6, 7].

---

## 📊 Dataset & Features

The dataset (`3_spotify_5000_songs.csv`) consists of **5,235 songs** across **2,176 unique artists** with Spotify audio metrics. 

### Preprocessing Steps
1. **Cleaning:** Stripped whitespace from column names and removed non-numerical metadata (`name`, `artist`, `html`, and empty `type` columns).
2. **Indexing:** Set the Spotify track `id` as the DataFrame index.
3. **Feature Selection & Scaling:** Selected **13 numerical features** (`danceability`, `energy`, `key`, `loudness`, `mode`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`, `duration_ms`, and `time_signature`) and applied `MinMaxScaler` so all features operate on a common `[0, 1]` scale[cite: 1, 3]. In the final presentation pipeline, overlapping/redundant features were distilled down to **7 key numerical indicators**[cite: 3].

---

## ⚙️ Methodology & Selecting `k` (Number of Playlists)

To find the optimal number of playlists ($k$), the model was evaluated across a range of $k = 1$ to $100$ clusters using two primary metrics:
* **Inertia (Elbow Method):** Measures within-cluster sum-of-squares (tightness of playlists)[cite: 1, 4].
* **Silhouette Score:** Measures how similar a song is to its own playlist compared to other playlists[cite: 1, 4].

### Business Constraints Meet Data Science
While mathematical peaks suggested fewer clusters, business logic dictated that a usable playlist should contain roughly **34 to 250 songs**[cite: 4]. Balancing the Silhouette Score peak within this usable playlist-size band led to selecting **$k = 43$ playlists**, resulting in an average of **122 songs per playlist**[cite: 4].

---

## 🔍 Results & Qualitative Evaluation

Inspecting the generated playlists revealed clear thematic groupings based on sound profiles, alongside interesting edge cases[cite: 5]:

| Playlist Example | Size | Vibe / Profile | Sample Tracks | Evaluation |
| :--- | :--- | :--- | :--- | :--- |
| **Playlist 5** | 192 songs[cite: 5] | **High Energy Hits & Dance Vibes**[cite: 5] | *Lush Life* (Zara Larsson), *Cheap Thrills* (Sia), *Toxic* (Britney Spears), *Waiting for Love* (Avicii)[cite: 5] | **Cohesive:** Danceable and lively; feels like a unified commercial pop/dance playlist[cite: 5]. |
| **Playlist 2** | 78 songs[cite: 5] | **Intense & Dark Energy**[cite: 5] | *Raining Blood* (Slayer), *Dig* (Mudvayne), *My Curse* (Killswitch Engage), *Arise* (Sepultura)[cite: 5] | **Mostly Cohesive:** Captures loud, fast, aggressive metal tracks—though an occasional high-energy outlier like *Calvin Harris* sneaks in due to similar tempo/loudness metrics[cite: 5]. |

### Strengths & Limitations of K-Means for Playlists
* **What the algorithm hears:** Loud vs. quiet, electric vs. acoustic, energetic vs. calm, and rap/spoken-word vs. singing[cite: 6].
* **What the algorithm misses:** Genre, language, lyrical themes, and release era[cite: 6].
* **Algorithm Trade-offs:** K-Means is fast on thousands of songs and guarantees every song gets assigned to a playlist[cite: 7]. However, $k$ must be chosen manually, cluster sizes cannot be strictly capped, and results can shift slightly between runs[cite: 7].

---

## 📂 Repository Structure

```text
├── 3_spotify_5000_songs.csv          # Raw Spotify dataset (5,235 tracks)
├── uml_playlist_creator.ipynb        # Data cleaning, MinMax scaling, Elbow/Silhouette analysis, & K-Means clustering
├── Moosic_playlist_prototype.pptx.pdf # Stakeholder presentation summarizing methodology, metrics, and recommendations
└── README.md                         # Project documentation
