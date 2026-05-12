# Chess Structure Clustering

Unsupervised machine learning project for discovering structural patterns in chess games using early-game features extracted from Lichess PGN games.

This project was created for **MTH 4224 – Intro to Machine Learning**.

## Project Overview

The goal of this project is not to predict the winner of a chess game.

Instead, this project studies whether chess games naturally form interpretable structural groups, such as:

- tactical games
- positional games
- balanced games
- dynamic or unstable games
- quieter structural games

Because chess games do not have true style labels, this is treated as an **unsupervised learning** problem.

The main question is:

> Do chess games naturally cluster into meaningful groups when described by engineered early-game features?

## Main Idea

Each chess game is converted into a numerical feature vector based on the first 20 full moves.

The features describe:

- tactical activity
- material changes
- Stockfish evaluation volatility
- pawn structure
- castling behavior
- center control
- king danger
- opening family

Several clustering methods are then applied to test whether natural groups appear in the feature space.

## Data

The dataset comes from publicly available games from the Lichess database.

- Source: Lichess PGN database
- Parsed games: 10,000
- Final dataset after preprocessing: 8,621 games
- Game segment used: first 20 full moves
- Learning type: unsupervised learning
- Outcome labels: not used
- Final feature matrix: 43 numerical features

Raw PGN files are not included in this repository because they can be large.

## Feature Engineering

The raw PGN files are processed using `python-chess`. Since clustering algorithms require numerical input, each game is converted into engineered chess features.

### Main Feature Groups

#### Dynamic Features

These features describe tactical activity and instability:

- capture values
- number of checks
- material volatility
- Stockfish evaluation volatility
- mean absolute engine evaluation

#### Structural Features

These features describe more positional aspects of the game:

- doubled pawns
- isolated pawns
- passed pawns
- pawn islands
- castling timing
- center control
- king danger
- minor piece development

#### Opening Features

Opening information is included through ECO code groups.

ECO/opening features are useful because different openings can lead to different structures. However, opening family is not the same thing as chess style, so these results must be interpreted carefully.

## Methods

This project compares several unsupervised learning methods.

### PCA

Principal Component Analysis is used for dimensionality reduction and visualization.

The first two principal components explain only about **13.92%** of total variance. This means the data structure is spread across many dimensions, and the 2D PCA projection does not show clean separated islands.

### K-Means Clustering

K-Means is used as the main clustering baseline because it is simple and interpretable through centroids.

Different values of `K` are tested using silhouette score.

The best K-Means result was around **K = 7**, but the silhouette score was still low, so this should not be interpreted as proof of seven natural chess styles.

### Gaussian Mixture Model

Gaussian Mixture Model is used as a soft clustering comparison. It allows probabilistic cluster assignments and helps check whether groups overlap.

### Agglomerative Clustering

Agglomerative clustering is used as a hierarchical comparison method. It does not depend on centroid initialization like K-Means.

### DBSCAN

DBSCAN is used to detect dense regions and sparse or unusual regions of the feature space.

DBSCAN is useful here because it does not force every game into a cluster. However, in this dataset it marks many games as sparse/noise, which supports the idea that chess games form a continuous feature space rather than clean dense groups.

## Results

### Full Feature Space Clustering

When all 43 features are used together, clustering separation is weak.

| Model | Setting | Silhouette Score |
|---|---:|---:|
| K-Means | K = 3 | 0.0935 |
| Gaussian Mixture Model | K = 3 | 0.0489 |
| Agglomerative Clustering | K = 3 | 0.0761 |
| DBSCAN | Core clusters | 0.0776 |

These scores suggest that the full feature space does not naturally split into clean chess-style groups.

This does not mean the project failed. Instead, this is one of the main findings: chess games appear more like a continuous spectrum than a small set of clearly separated categories.

## Feature Group Comparison

The feature group comparison was more informative than clustering all features together.

| Feature Group | Number of Features | K-Means Silhouette |
|---|---:|---:|
| Dynamic Features | 7 | 0.2777 |
| ECO / Opening Features | 20 | 0.2487 |
| Structural Features | 16 | 0.1106 |
| All Features | 43 | 0.0935 |

### Interpretation

Dynamic features produced the strongest clustering signal. This means that captures, checks, material volatility, and evaluation volatility separate games better than the full mixed feature set.

ECO/opening features also produced strong structure, but this should be interpreted as opening-family structure, not necessarily pure playing style.

Structural features were weaker, but still useful for interpretation.

## Main Findings

1. Chess games do not form clean separated clusters in the full engineered feature space.

2. Dynamic features give the strongest clustering signal.

3. ECO/opening family also creates strong structure, but it should not be confused with playing style.

4. Structural features such as pawn structure, king danger, castling, and center control are useful, but they do not separate games as strongly as dynamic features.

5. DBSCAN is not a strong general clustering model for this dataset because most games are not inside broad dense clusters.

6. Chess styles are better understood as tendencies on a spectrum, not strict natural labels.

## Repository Structure

<pre>
.
├── README.md
├── LICENSE
├── NOTICE.md
├── requirements.txt
├── notebooks/
│   └── chess_structure_clustering.ipynb
├── reports/
│   └── MTH4224_Project2_Report.pdf
├── slides/
│   └── MTH4224_Project2_Slides.pdf
└── figures/
    ├── pca_projection.png
    ├── kmeans_silhouette.png
    ├── kmeans_stability.png
    ├── centroid_heatmap.png
    └── dbscan_sparse_noise.png
</pre>

The `figures/` folder is optional. It can be used if exported plots are included separately from the notebook.

## Installation

Clone the repository:

<pre>
git clone https://github.com/Darhaal/chess-structure-clustering.git
cd chess-structure-clustering
</pre>

Create and activate a virtual environment:

<pre>
python -m venv venv
</pre>

On Windows:

<pre>
venv\Scripts\activate
</pre>

On macOS/Linux:

<pre>
source venv/bin/activate
</pre>

Install dependencies:

<pre>
pip install -r requirements.txt
</pre>

## Requirements

Main Python libraries used:

<pre>
numpy
pandas
matplotlib
scikit-learn
python-chess
scipy
jupyter
</pre>

If Stockfish-based evaluation features are recomputed, Stockfish must be downloaded separately and its path must be configured in the notebook.

Stockfish binary is not included in this repository.

## How to Run

Open the notebook:

<pre>
jupyter notebook notebooks/chess_structure_clustering.ipynb
</pre>

Then run the notebook cells in order.

The general pipeline is:

1. Load or parse Lichess PGN games.
2. Extract early-game chess features.
3. Clean and preprocess the dataset.
4. Apply scaling and transformations.
5. Run PCA.
6. Train clustering models.
7. Compare silhouette scores.
8. Analyze feature groups.
9. Interpret clusters using centroid analysis.
10. Summarize limitations and findings.

## Important Notes

This project is exploratory.

The clusters should not be treated as exact labels such as "tactical", "positional", or "balanced". The results show tendencies in the feature space, not strict chess categories.

Low silhouette scores are part of the conclusion. They show that chess games do not naturally separate into a few clean groups when all engineered features are used together.

## Limitations

There are several limitations:

- Features are manually engineered and cannot capture all chess ideas.
- Long-term planning, piece coordination, and deep positional compensation are hard to encode numerically.
- Stockfish evaluations are sampled only at selected positions to keep computation feasible.
- Outlier filtering removes some unusual games that may be real tactical examples.
- ECO/opening features may reflect opening choice rather than actual playing style.
- There are no ground-truth labels for chess style, so evaluation depends on both metrics and interpretation.

## Future Work

Possible extensions:

- Use a larger dataset.
- Extract features from more phases of the game.
- Compare early-game, middlegame, and endgame structures.
- Use deeper Stockfish evaluation or more evaluation points.
- Add graph-based board representations.
- Use autoencoders or representation learning instead of only manual features.
- Build an interactive visualization of clusters.
- Compare clusters with player rating, time control, or opening family.

## Tech Stack

- Python
- pandas
- NumPy
- scikit-learn
- python-chess
- Stockfish
- matplotlib
- Jupyter Notebook

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Notice

This repository is an academic and portfolio project.

Raw Lichess PGN database files and the Stockfish engine binary are not included in this repository.

Please do not submit this project as your own academic work.
