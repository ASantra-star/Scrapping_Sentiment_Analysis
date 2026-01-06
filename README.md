
# Automated Web Article Sentiment & Readability Analysis

## Overview

This project performs **end-to-end text analytics** on web articles automatically fetched from multiple RSS feeds.
It extracts article content, computes **sentiment analysis**, **readability metrics**, and **linguistic complexity measures**, and evaluates **lexicon-based sentiment (VADER)** against a **modern transformer-based NLP model**.

The system is fully automated and scalable, handling **200+ articles** without manual input files.

---

## Objectives

1. Automatically collect article URLs from the web
2. Scrape and clean article text
3. Compute sentiment and readability metrics
4. Generate original and extended analytical datasets
5. Visualize insights (word cloud, complexity ranking)
6. Validate sentiment accuracy using a transformer model

---

## Data Sources

Articles are collected from publicly available RSS feeds, including:

* BBC Technology
* Reuters Technology
* New York Times Technology
* Wired
* Ars Technica
* TechCrunch
* The Verge
* MIT Technology Review
* Engadget
* CNET

These sources ensure sufficient article length and high-quality content.

---

## Pipeline Architecture

### Step 1: URL Collection

* RSS feeds are parsed using `feedparser`
* URLs are deduplicated
* Articles with fewer than 50 words are discarded
* A minimum of **200 valid articles** is enforced

### Step 2: Web Scraping

* Article text is extracted from `<p>` tags using `BeautifulSoup`
* User-Agent headers are used to avoid request blocking
* Extracted text is stored in the dataset for reuse

### Step 3: Text Cleaning

* Tokenization using NLTK
* Stopword removal
* Punctuation and numeric filtering
* Syllable estimation for complexity analysis

---

## Output Files

### 1. Input.xlsx

Contains automatically generated URLs:

```
URL_ID | URL
```

### 2. Output.xlsx (Original Metrics)

Includes baseline sentiment and readability metrics:

* Positive Score
* Negative Score
* Polarity Score
* Subjectivity Score
* Average Sentence Length
* Percentage of Complex Words
* Fog Index
* Word Count
* Syllables per Word
* Personal Pronouns
* Average Word Length

### 3. Output_Extended.xlsx / CSV

Includes all original metrics plus advanced features:

* Article Text
* Domain
* Neutral Score
* Sentiment Intensity
* Emotionality Score
* Unique Word Count
* Type-Token Ratio
* Sentence Count
* Paragraph Count
* Flesch Reading Ease
* Reading Time
* Question Count
* Exclamation Count
* Article Length Category

---

## Sentiment Analysis Methods

### VADER (Lexicon-Based)

* Uses NLTK’s `SentimentIntensityAnalyzer`
* Strong sentiment thresholds:

  * Positive: compound > 0.05
  * Negative: compound < -0.05
  * Neutral: otherwise

### Transformer-Based Model

* Model: `distilbert-base-uncased-finetuned-sst-2-english`
* Implemented via Hugging Face `transformers`
* Outputs label and probability score
* Serves as a modern benchmark

---

## Sentiment Evaluation

* VADER sentiment labels are compared against transformer predictions
* Metrics computed:

  * Accuracy
  * Precision
  * Recall
  * F1-score
* Mismatch analysis identifies cases where lexicon-based methods fail

Key finding:
Lexicon-based sentiment analysis performs well for positive sentiment but struggles with negative sentiment in long-form articles.

---

## Visualizations

### Word Cloud

* Generated from combined article text
* Highlights dominant topics and terminology

### Complexity Analysis

* Articles ranked by:

  * Complex Word Count
  * Fog Index
* Top 10 most complex articles identified

### Scatter & Distribution Plots

* VADER compound score vs transformer probability
* Readability and lexical diversity distributions

---

## Dependencies

Install all dependencies using:

```bash
pip install pandas feedparser requests beautifulsoup4 nltk openpyxl \
            wordcloud seaborn matplotlib transformers torch tqdm
```

