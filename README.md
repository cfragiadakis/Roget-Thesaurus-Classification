# Roget's Thesaurus Classification


In this project, we will analyze the [Roget's Thesaurus](https://www.gutenberg.org/files/10681/old/20040627-10681-h-body-pos.htm#180.1) lexicon.
Through web scraping, we access Roget's comprehensive collection of words and their associations. We will implement word embeddings for the word entries of the lexicon, aiming to achieve two objectives:
* Explore if there is a clustering method that can produce similar hierarchies as Roget's.
* Develop the best classification method, wherein the model's task is to discern a word's correct Class or Section/Division within Roget's categorization.

3rd Assignment for Applied Machine Learning 2024 (DMST, AUEB)

You can find the full description of the assignment [here.](https://github.com/cfragiadakis/Roget-s-Thesaurus-Classification/blob/main/third_assignment.ipynb) 

# Main Sections

## Web Scraping Roget's lexicon

We use `requests` and `beautifulsoup` libraries to acquire the lexicon of the words across section/division level, storing it to a `pandas` dataframe.

## Preprocessing lexicon

On this section, we apply preprocessing steps in order to convert our lexicon across word level and also apply steps:
* Remove punctuations, numbers, whitespaces.
* Remove words that represent parts of speech (e.g. 'adj', 'n', 'v', 'adv'...).
* Remove phrases part of each section/division of lexicon since this part of text does not add additional words to our lexicon. 

## Class & Section/Division Analysis

In this section, we generate visualizations to identify the most and least well-represented classes,  as well as to highlight any imbalances in the lexicon across different class and section/division levels.

## Word Embeddings

We generate word embeddings using two models, one leveraging the OpenAI API and another utilizing the mxbai-embed-large model, a completely free approach. For the next tasks, we use only the first since they yield slightly superior results.

In both approaches, considering the significant time required for this step, we account for instances of network errors (when using the OpenAI API) or any other issues that may arise. Consequently, we save each word's embedding in a file to prevent loss of progress. Subsequently, we concatenate all saved embeddings and store them in a JSON file.


## Clustering

## Class/Section Prediction
