# Steam User Engagement Analysis

An analysis of large-scale Steam game and user review data to identify user-defined tag combinations associated with high engagement and assess whether similar tag patterns appear in recently released games.

**Python · Dask · pandas · Apriori · Jaccard Similarity · Large-Scale Data Analysis**

## Project Overview

Steam users apply tags to games based on characteristics such as gameplay, content, genre, mood and playstyle. This project investigates whether recurring combinations of these user-defined tags are associated with highly engaged games and whether the identified patterns can be used to identify recently released games with similar characteristics.

Large-scale game metadata and user review data were processed using Python and Dask. Frequent tag combinations among high-engagement games were identified using the Apriori algorithm, while Jaccard similarity was used to compare these patterns with the tags of recently released games.

### Research Question

**Which user-defined tags are most commonly associated with high user engagement, and can these tags be used to identify emerging games with similar patterns?**

## Data

The project uses **Version 2 of the Steam Video Game and Bundle Data**, combining:

- Game metadata, including user-defined tags and release dates
- User review data, including hours played
- Review counts and aggregated playtime as measures of engagement

The metadata and review datasets were cleaned, filtered and merged using the game `product_id`.

## Methodology

### 1. Data Preprocessing

The large raw datasets were processed using **Dask** and **pandas**. The preprocessing workflow included:

- Decompressing and reading the raw game and review data
- Selecting relevant metadata and engagement variables
- Normalising user-defined tags
- Converting release dates to datetime format
- Merging game metadata and review data by `product_id`
- Aggregating total hours played and number of reviews for each game

### 2. Identifying High-Engagement Games

Games were classified as high engagement when both their:

- total hours played, and
- number of user reviews

were above their respective 75th percentile thresholds.

A separate subset of recently released games was created using games released within two years of the most recent release date in the dataset.

### 3. Frequent Tag Pattern Mining

User-defined tag lists for high-engagement games were transformed into transaction data and analysed using the **Apriori algorithm**.

Frequent tag combinations with support greater than **0.2** were retained to identify recurring tag patterns among highly engaged games.

### 4. Tag Similarity Analysis

**Jaccard similarity** was used to compare the tags of recently released games against the frequent tag combinations identified among high-engagement games.

Each recent game was assigned its maximum Jaccard similarity score. A score of **1.0** represented an exact match between a recent game's tags and an identified frequent tag combination.

## Key Findings

- **38 frequent tag combinations** with support greater than 0.2 were identified among high-engagement games.

- The five most frequent tag combinations were:
  1. `singleplayer + action`
  2. `adventure + singleplayer`
  3. `singleplayer + indie`
  4. `adventure + action`
  5. `multiplayer + action`

- **14 recently released games achieved a Jaccard similarity score of 1.0**, indicating exact matches with frequent tag combinations identified among high-engagement games.

- The results suggest that recurring combinations of community-defined tags can provide a useful signal for identifying newer games that share characteristics with previously high-engagement titles.

## Scalability

Dask was used to support processing of the large Steam datasets, including scalable reading and preprocessing of the data and partition-based application of the Jaccard similarity calculations.

A scalability test showed increasing runtime as larger proportions of the review dataset were processed, highlighting both the computational demands of the dataset and opportunities for further optimisation.

## Repository Structure

```text
steam-user-engagement/
│
├── README.md
│
│── analysis/
│   └── steam-user-engagement.ipynb
│
└── docs/
    └── final-report.pdf
```

## Limitations

The analysis identifies associations between user-defined tag patterns and engagement rather than establishing that particular tags cause higher engagement.

The approach also uses tag similarity as an indicator of potential engagement for recently released games rather than evaluating future popularity using observed outcomes.

Data preprocessing represented a computational bottleneck because some raw records required sequential conversion before they could be processed efficiently with Dask.

## Future Improvements

Potential extensions include:

- Incorporating additional game metadata such as developer-defined genres
- Examining whether tag patterns differ across user groups or regions
- Improving preprocessing through greater parallelisation
- Exploring distributed computing approaches beyond Dask
- Evaluating identified emerging games against subsequent observed engagement

## Documentation

For a detailed description of the methodology, results and project evaluation, see the [Final Report](docs/final-report.pdf).

---

*Originally completed as an individual project for DATA301: Big Data Computing and Systems at the University of Canterbury.*
