# Chess Structure Clustering

Unsupervised machine learning project for discovering structural patterns in chess games using early-game features extracted from Lichess PGN games.

This project was completed at Florida Institute of Technology for MTH 4224 – Intro to Machine Learning.

## Repository Description

Unsupervised learning project discovering structural patterns in chess games using clustering and engineered early-game features.

## Project Overview

The goal of this project is not to predict the winner of a chess game.

Instead, this project studies whether chess games naturally form interpretable structural groups, such as:

- tactical games
- positional games
- balanced games
- dynamic or unstable games
- quieter structural games

Because chess games do not have true style labels, this is treated as an unsupervised learning problem.

The main question is:

Do chess games naturally cluster into meaningful groups when described by engineered early-game features?

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

The raw PGN files are processed using python-chess.

Since clustering algorithms require numerical input, each game is converted into engineered chess features.

Main feature groups include:

Dynamic features:

- capture values
- number of checks
- material volatility
- Stockfish evaluation volatility
- mean absolute engine evaluation

Structural features:

- doubled pawns
- isolated pawns
- passed pawns
- pawn islands
- castling timing
- center control
- king danger
- minor piece development

Opening features:

- ECO opening group
- one-hot encoded opening-family information

ECO/opening features are useful because different openings can lead to different structures. However, opening family is not the same thing as chess style, so these results must be interpreted carefully.

## Methods

This project compares several unsupervised learning methods:

- PCA
- K-Means Clustering
- Gaussian Mixture Model
- Agglomerative Clustering
- DBSCAN

PCA is used for dimensionality reduction and visualization.

K-Means is used as the main clustering baseline because it is simple and interpretable through centroids.

Gaussian Mixture Model is used as a soft clustering comparison.

Agglomerative Clustering is used as a hierarchical comparison method.

DBSCAN is used to detect dense regions and sparse or unusual regions of the feature space.

## Results

When all 43 features are used together, clustering separation is weak.

Full feature-space clustering results:

K-Means:
- Setting: K = 3
- Silhouette score: 0.0935

Gaussian Mixture Model:
- Setting: K = 3
- Silhouette score: 0.0489

Agglomerative Clustering:
- Setting: K = 3
- Silhouette score: 0.0761

DBSCAN:
- Setting: core clusters
- Silhouette score: 0.0776

These scores suggest that the full feature space does not naturally split into clean chess-style groups.

This does not mean the project failed. Instead, this is one of the main findings: chess games appear more like a continuous spectrum than a small set of clearly separated categories.

## Feature Group Comparison

The feature group comparison was more informative than clustering all features together.

Dynamic Features:
- Number of features: 7
- K-Means silhouette: 0.2777

ECO / Opening Features:
- Number of features: 20
- K-Means silhouette: 0.2487

Structural Features:
- Number of features: 16
- K-Means silhouette: 0.1106

All Features:
- Number of features: 43
- K-Means silhouette: 0.0935

## Interpretation

Dynamic features produced the strongest clustering signal.

This means that captures, checks, material volatility, and evaluation volatility separate games better than the full mixed feature set.

ECO/opening features also produced strong structure, but this should be interpreted as opening-family structure, not necessarily pure playing style.

Structural features were weaker, but still useful for interpretation.

## Main Findings

1. Chess games do not form clean separated clusters in the full engineered feature space.

2. Dynamic features give the strongest clustering signal.

3. ECO/opening family also creates strong structure, but it should not be confused with playing style.

4. Structural features such as pawn structure, king danger, castling, and center control are useful, but they do not separate games as strongly as dynamic features.

5. DBSCAN is not a strong general clustering model for this dataset because most games are not inside broad dense clusters.

6. Chess styles are better understood as tendencies on a spectrum, not strict natural labels.

## Files in This Repository

All project files are stored in the root of the repository.

Expected files:

- README.md
- LICENSE
- NOTICE.md
- MTH4224 Project2 Jupyter.ipynb
- MTH4224_Project2_Report.pdf
- MTH4224_Project2_Slides.pdf

## How to View the Project

The easiest way to review the project is to open the PDF report or the Jupyter notebook directly on GitHub.

The notebook contains the code, outputs, plots, and analysis.

The report gives the full written explanation of the project.

The slides summarize the project for presentation.

## Reproducibility Notes

This repository is mainly shared as an academic and portfolio project.

To fully rerun the notebook, the user may need to manually install the Python libraries used inside the notebook.

Stockfish must be downloaded separately if engine-based features are recomputed.

Raw Lichess PGN files are not included because they are large.

## Important Notes

This project is exploratory.

The clusters should not be treated as exact labels such as tactical, positional, or balanced. The results show tendencies in the feature space, not strict chess categories.

Low silhouette scores are part of the conclusion. They show that chess games do not naturally separate into a few clean groups when all engineered features are used together.

## Limitations

There are several limitations:

- Features are manually engineered and cannot capture all chess ideas.
- Long-term planning, piece coordination, and deep positional compensation are hard to encode numerically.
- Stockfish evaluations are sampled only at selected positions to keep computation feasible.
- Outlier filtering removes some unusual games that may be real tactical examples.
- Opening-family structure can overlap with playing-style interpretation.
- There are no ground-truth labels for chess style.

## License

This project is licensed under the MIT License.
