# Roget's Thesaurus Classification


This project delves into the analysis of [Roget's Thesaurus](https://www.gutenberg.org/files/10681/old/20040627-10681-h-body-pos.htm#180.1) lexicon.
Roget's comprehensive collection of words and their associations is accessed through web scraping. Next, word embeddings are implemented for the word entries of the lexicon, aiming to achieve two objectives:
* Explore if there is a clustering method that can produce similar hierarchies to Roget's.
* Develop the best classification method, wherein the model's task is to discern a word's correct Class or Section/Division within Roget's categorization.

# Installation

## Dependencies

You can install the dependencies of the project using [Poetry](https://python-poetry.org/):

```
poetry install
```

or using pip:

```
pip install -r requirements.txt
```
## Data Resources

You can access the word embeddings used in this project, as well as the machine learning classification models for the "Class/Section Prediction" section through this [link](https://drive.google.com/drive/folders/1fhWq8VNi8ZOD0D2jUteGo__cet208hvB?usp=sharing)

# Main Sections

## Web Scraping Roget's lexicon

`requests` and `beautifulsoup` libraries are used to acquire the lexicon of the words across section/division level, storing it to a `pandas` dataframe.

## Preprocessing lexicon

Preprocessing steps are applied to convert our lexicon across word level, along with:
* Removing punctuations, numbers, whitespaces.
* Removing words that represent parts of speech (e.g. 'adj', 'n', 'v', 'adv'...).
* Removing phrases part of each section/division of lexicon since this part of text does not add additional words to our lexicon. 

After these steps, the lexicon comprises around **73,000** words, categorized into **6** classes and further subdivided into **24** sections/divisions.

## Class & Section/Division Analysis

In this section, a few plots are implemented to identify the most and least well-represented classes,  as well as to highlight any imbalances in the lexicon across different class and section/division levels.

## Word Embeddings

Word embeddings are generated with two methods, one leveraging the [OpenAI API](https://platform.openai.com/docs/guides/embeddings), and a completely free approach, utilizing the [mxbai-embed-large model](https://huggingface.co/mixedbread-ai/mxbai-embed-2d-large-v1). For the next tasks, only the first are used since they yield slightly superior results.

In both approaches, considering the significant time required for this step, instances of network errors (when using the OpenAI API) or any other issues that may arise, are considered. Consequently, each word's embedding is saved in a separate file to prevent loss of progress. Subsequently, all saved embeddings are concatenate and stored in a JSON file.


## Clustering
Clustering at class level is implemented to explore whether clustering algorithms can produce clusters similar to Roget's classification.

Before applying any algorithms, we visualize the actual representation of Roget's classes using PCA (Principal Component Analysis) with 2 components, along with the centroids of each class.

The same visualization approach was applied to the representations generated by the following clustering algorithms:

* K-Means
* Gaussian Mixture
  
6 clusters are selected, corresponding to the number of classes in Roget's lexicon. Techniques such as PCA (with the number of components that explain 80% of the variance) and data standardization are also applied.

The evaluation metrics for assessing the results include metrics from the classification report and the Adjusted Rand Index (ARI). These indicators provide insights into the similarity between the produced clusters and Roget's classification.

## Class/Section Prediction

Utilizing `scikit-learn` and `lightgbm` libraries, the aim is to develop two models that receive a word as input, and predict their class or section/division.

To split the dataset into training, validation, and testing sets, we implement a function that ensures each dataset has the same representation of each class/section.

For both tasks, the initial tests include the use of multiple vanilla models, including Logistic Regression, SGD Classifier, Gaussian NB, Random Forest Classifier, XGBoost, and LGBM Classifier.

Each model's performance is visualized with a confusion matrix. 

### Class Prediction

In the class prediction task, which involves 6 possible labels, `scikit-optimize` and Bayesian Optimization for hyperparameter tuning are used. Also, ensemble methods further improve the results. The final class model is a Voting Classifier, consisting of a Logistic Regression and an LGBM Classifier, achieving 62% accuracy.

### Section/Division Prediction

Similar steps are applied for the Section/Division prediction, which involves 24 possible labels. Grid search is implemented for the Logistic Regression model, which achieves superior results compared to other models. In this case, the Bagging ensemble method also slightly increases the precision of the model. The final section model achieves 49% accuracy, which is reasonable considering the significantly larger number of possible labels compared to the class prediction task.


--- 

3rd Assignment for Applied Machine Learning 2024 (DMST, AUEB)

You can find the description of the assignment [here](https://github.com/cfragiadakis/Roget-s-Thesaurus-Classification/blob/main/third_assignment_description.ipynb).
