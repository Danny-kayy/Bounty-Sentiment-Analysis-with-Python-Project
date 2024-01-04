# Bounty Sentiment Analysis with Python Project

## Step 1: Introduction

**What is sentiment analysis?**

Sentiment analysis is a branch of natural language processing and data analysis dedicated to discerning the sentiment conveyed by a sequence of words. Its goal is to comprehend the attitudes, opinions, and emotions expressed in online mentions. Sentiment analysis seeks to categorize the emotional tone of a text, determining whether the opinions conveyed in documents, tweets, reviews, or comments are positive, negative, or neutral.

To run the code and install necessary libraries, use the following command:
```bash
pip install pandas matplotlib seaborn nltk scikit-learn

## Step 2: Introduction to VADER

[VADER](https://github.com/cjhutto/vaderSentiment) is a sentiment analysis tool designed to effectively capture sentiments expressed in social media. It is accessible through the Natural Language Toolkit (NLTK) library for Python. The tool assesses text, analyzing the sentiment of individual lexical features. It refines sentiment scores by applying rules that take into account syntax and grammatical conventions, ultimately offering an overall sentiment score.

### Set up VADER:

To use VADER, make sure to install the 'nltk' library and download the 'vader_lexicon':

```python
import nltk

# Uncomment the code below and run it to download vader_lexicon
# nltk.download('vader_lexicon')

from nltk.sentiment import SentimentIntensityAnalyzer
sia = SentimentIntensityAnalyzer()

## Step 3: Data Acquisition

With our setup complete, we're now equipped to get data for our project.

### Data Acquisition:

I'll be using web scraping technique 'beautifulsoup' to extract data easily. In this project, IMDb movie reviews will be used for sentiment analysis.

```python
import requests
from bs4 import BeautifulSoup

# URL of the IMDb page
my_url = 'https://www.imdb.com/title/tt10872600/reviews/?ref_=tt_ov_rt'

# Fetch the web page content
response = requests.get(my_url)
html_content = response.content

# Create a Beautiful Soup object
soup = BeautifulSoup(html_content, 'html.parser')

# Find the HTML element containing reviews (adjust according to the actual HTML structure)
reviews_elements = soup.find_all('a', class_='title')

# Extract and print the text content of each review
for review_element in reviews_elements:
    review_text = review_element.text.strip()
    print(review_text)
    print('-' * 50)
## Step 4: Sentiment Analysis

Next, create a sentiment analysis classifying each viewer's review as either positive, negative, or neutral based on the compound sentiment score.

```python
def scrape_reviews(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    # Extract reviews from the IMDb page
    viewers_reviews = [review.text for review in soup.find_all('a', {'class': 'title'})]
    return viewers_reviews

def analyze_sentiment(review):
    sid = SentimentIntensityAnalyzer()
    sentiment_score = sid.polarity_scores(review)

    # Classify the sentiment based on the compound score
    if sentiment_score['compound'] >= 0.05:
        return 'Positive statement'
    elif sentiment_score['compound'] <= -0.05:
        return 'Negative statement'
    else:
        return 'Neutral statement'

# IMDb movie reviews URL
imdb_url = 'https://www.imdb.com/title/tt10872600/reviews/?ref_=tt_ov_rt'

# Scrape reviews from the IMDb page
reviews = scrape_reviews(imdb_url)

# Analyze sentiment for each review and print the results
for i, review in enumerate(reviews, start=1):
    sentiment = analyze_sentiment(review)
    print(f'Review #{i}:\n{review}\nSentiment: {sentiment}\n{"-"*50}\n')
## Step 5: Visualizing Results

Present the outcomes in a simple bar chart format.

```python
import matplotlib.pyplot as plt

# ... (previous code)

# Analyze sentiment for each review and collect the results
sentiments = [analyze_sentiment(review) for review in reviews]

# Count the occurrences of each sentiment
sentiment_counts = {
    'Positive': sentiments.count('Positive'),
    'Negative': sentiments.count('Negative'),
    'Neutral': sentiments.count('Neutral')
}

# Plot the results in a bar chart
labels = list(sentiment_counts.keys())
counts = list(sentiment_counts.values())

plt.bar(labels, counts, color=['green', 'red', 'yellow'])
plt.xlabel('Sentiment')
plt.ylabel('Number of Reviews')
plt.title('Sentiment Analysis of IMDb Reviews')
plt.show()

# Step 6: Summary

The provided Python code conducts sentiment analysis on IMDb movie reviews using web scraping and Natural Language Toolkit (NLTK) with the VADER sentiment analysis tool. The goal is to extract reviews from the IMDb page of a specific movie and determine whether each review is positive, negative, or neutral.

## Script Overview

The script starts by importing necessary libraries: `requests` for making HTTP requests, `BeautifulSoup` for parsing HTML content, and `SentimentIntensityAnalyzer` from NLTK for sentiment analysis. The `scrape_reviews` function utilizes Beautiful Soup to extract reviews from the IMDb page by locating elements with the specified class. The `analyze_sentiment` function employs VADER to assign a sentiment score to each review and classifies them based on a compound score threshold.

## Execution

The main script then applies these functions to the IMDb page, printing each review along with its sentiment analysis result. This approach offers a straightforward sentiment analysis using pre-built tools, making it accessible for individuals seeking a quick sentiment assessment of IMDb movie reviews.

## Conclusion

The sentiment analysis results are presented in a bar chart, showing the number of reviews classified as positive, negative, or neutral. This visual representation provides a clear overview of the sentiment distribution among the IMDb movie reviews for the specified film. Users can adapt and modify the script for their specific movie of interest by changing the IMDb URL in the data acquisition section.
