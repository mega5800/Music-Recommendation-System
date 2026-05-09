# 🎵 Music Recommendation System

A graph-based music recommendation engine that uses **Node2Vec** embeddings and **K-Nearest Neighbors** to suggest similar songs across multiple query types — by song title, artist, genre, decade, country, or year.

---

## ⚙️ How It Works

1. **Data Preprocessing** — Loads and merges music/artist datasets, removes missing values, normalizes numeric features, and enriches each song with a geographic country (derived from artist lat/lon coordinates via GeoJSON lookup).
2. **Knowledge Graph Construction** — Builds an undirected NetworkX graph where 1,000 song nodes connect to 7 decade hub nodes (1950s–2010s). Numeric attributes (tempo, loudness, duration, etc.) are stored as node features after min-max normalization.
3. **Node2Vec Embedding** — Runs biased random walks on the graph to produce 64-dimensional vector representations for every node (songs + decade hubs).
4. **KNN Model** — Trains a K-Nearest Neighbors model (k=6) on the embeddings to identify the most similar songs in embedding space.
5. **Interactive Query Interface** — Accepts a query type and value, finds matching songs in the dataset, and returns the 6 most similar recommendations.

---

## 🔍 Query Types

| # | Query Type  | Example value      |
|---|-------------|-------------------|
| 1 | Song title  | `"Hotel California"` |
| 2 | Artist name | `"The Beatles"`     |
| 3 | Decade      | `"1990s"`           |
| 4 | Country     | `"United States"`   |
| 5 | Term (genre)| `"rock"`            |
| 6 | Year        | `"1975"`            |

---

## 📁 Project Structure

```
Music-Recommendation-System/
├── Code/
│   └── Music_Recommendation_System.ipynb   # Full pipeline: preprocessing → graph → embeddings → recommendations
├── Music Data/
│   └── music_dataset.csv                   # Processed dataset (1,000 songs, 13 columns)
├── models/
│   ├── embedding                           # Pre-trained 64-dim Node2Vec embeddings (Word2Vec format)
│   └── node2vec_model                      # Saved Node2Vec model
└── .gitignore
```

---

## 📊 Dataset

The processed dataset (`music_dataset.csv`) contains **1,000 songs** with the following columns:

| Column               | Description                              |
|----------------------|------------------------------------------|
| `song_title`         | Song name                                |
| `year`               | Release year                             |
| `release`            | Album / release name                     |
| `tempo`              | Beats per minute                         |
| `loudness`           | Audio loudness (dB)                      |
| `duration`           | Length in seconds                        |
| `song_hotttnesss`    | Song popularity score (0–1)              |
| `artist_name`        | Artist or band name                      |
| `artist_hotttnesss`  | Artist popularity score (0–1)            |
| `artist_familiarity` | How widely known the artist is (0–1)     |
| `term`               | Musical genre / style                    |
| `country`            | Artist's country of origin               |
| `decade`             | Decade of release (e.g. `1990s`)         |

---

## 🕸️ Knowledge Graph

- **1,007 nodes** — 1,000 song nodes + 7 decade nodes
- **1,000 edges** — each song connected to its decade hub
- Numeric features are min-max normalized before embedding to prevent any single feature from dominating distances

---

## 🧠 Node2Vec Configuration

| Parameter     | Value |
|---------------|-------|
| Dimensions    | 64    |
| Walk length   | 10    |
| Walks per node| 100   |
| Window size   | 10    |
| Workers       | 4     |

---

## 📦 Installation

Install with:

```bash
pip install shapely node2vec networkx numpy pandas gensim scikit-learn scipy matplotlib tqdm smart-open
```

---

## 🚀 Getting Started

1. Clone the repository:
   ```bash
   git clone https://github.com/mega5800/Music-Recommendation-System.git
   cd Music-Recommendation-System
   ```

2. Install dependencies (see above).

3. Open and run the notebook:
   ```bash
   jupyter notebook Code/Music_Recommendation_System.ipynb
   ```

4. The notebook will:
   - Load the preprocessed dataset from `Music Data/music_dataset.csv`
   - Reconstruct the knowledge graph
   - Load (or re-train) the Node2Vec embeddings from `models/`
   - Launch an interactive prompt where you can enter a query type and value to receive song recommendations

---
