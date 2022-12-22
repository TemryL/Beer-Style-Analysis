# BeerVise: Pick the right beer for you!

Have you ever been stuck in front of your favourite supermarket beers' shelves asking yourself which beer you should choose? Shall you try a new style of beer or keep up with the same one you have been drinking for several years? And of course, the way the beers are organised on the shelves doesn't help much right? Well, we have the solution for you, or shall we say, the solution for your supermarket which will better satisfy your needs, and this story is all about it ...

## 📖 Find our Data Story [here](https://temryl.github.io/BeerVise/)
The Data Story we created for this project can be found at the following URL: https://temryl.github.io/BeerVise/. 
We hope you enjoy reading it!

## 👨‍💻 Scroll up and you'll find our project's notebook 
The complete notebook of our project can be found in our repository, it is called: `milestone3.ipynb`. For a better reading experience, please download the given notebook. 

## 📝 Abstract 
Nowadays, drinking beer has become a global social habit, whether it be for parties, football games or meeting with friends. In order to answer this demand, the market has become very attractive and diverse with breweries all around the world producing a wide variety of beer styles. This wide range of offer makes it more difficult for costumers to find a style of beer they might like. Being aware of this issue and with the aim to improve their customers’ satisfaction, an international supermarket chain called upon our data scientists’ team to help better organize their beer shelves. Our strategy is to associate each row of the shelves to a customers’ type. Then, for each of these customers’ categories, the most appreciated styles of beers are promoted with a textual description of their main features. Besides, in order to satisfy the firms’ multidomestic strategy, the arrangement of the beer shelves accounts for a 'nationalist' category wich showcases the best beers of the country we are in. 

## ❓ Research questions 
1. How can beer styles be merged into supra-styles? Which beer styles have features in common and can therefore be grouped (e.g. abv, countries or breweries where they are produced…)?
3. From the reviews, and in order to present a textual description of the beer styles on the shelves, what are the adjectives that best describe each beer supra style?
4. According to which metric can the users be categorised, and how to determine to which categories each user belongs to?
5. Is the taste of a user influenced by his country of origin or his expertise?

## 💾 Data
For this project two dataset have been provided: data from [BeerAdvocate](https://www.beeradvocate.com) as well as from [RateBeer](https://www.ratebeer.com/) websites.
Both dataset are organized following the same relational model (see `ER_diagram.png` and `relations.png` in figures folder). The following analysis will be done on each dataset separately and the results will be compared at the end. Raw data are available [here](https://drive.google.com/drive/folders/1Wz6D2FM25ydFw_-41I9uTwG9uNsN4TCF).
Processed data are available [here](https://drive.switch.ch/index.php/s/QBPV4ptiUoV8XER). In order to reproduce the analysis a data folder with the following architecture should be placed in the root directory of the repository:

<pre>  
├─── data
    ├─── raw
      ├─── BeerAdvocate
        ├─── beers.csv
        ├─── breweries.csv
        ├─── ratings.txt
        ├─── reviews.txt
        ├─── users.csv
      ├─── RateBeer
        ├─── beers.csv
        ├─── breweries.csv
        ├─── ratings.txt
        ├─── reviews.txt
        ├─── users.csv
    ├─── processed
      ├─── BeerAdvocate
        ├─── beers.pkl
        ├─── breweries.pkl
        ├─── ratings.pkl
        ├─── reviews.pkl
        ├─── users.pkl
      ├─── RateBeer
        ├─── beers.pkl
        ├─── breweries.pkl
        ├─── ratings.pkl
        ├─── reviews.pkl
        ├─── users.pkl 
</pre>

If one wants to manually re-produced the processed data from the raw data, simply move to the `src` folder and execute:

```
python process_raw_data.py
```

Make sure to have all the requirements and that the data in the raw data folder have been uncompressed. Be aware that running the script took around 20 min on Apple silicon M1 Pro. 

Note that after processing the raw data, we finally only worked on the [BeerAdvocate](https://www.beeradvocate.com) dataset as the results we were obtaining with this unique dataset were sufficient and already satisfying.

## 📊 Methods 

**Task 1**: Data exploratory, data cleaning and pre-processing.

This first step correponds to the data processing that is explained in the above [Data](#data) section in more details. Furthermore, it also encompasses the different datasets exploratory in particular analysing the number of breweries, users, ratings and reviews per country. Similarly the number of different beers and beer styles produced per country is usefull to have. For each country, we can also state the proportion of beer styles of beers from a certain country that have been rated.


**Task 2**: Descriptive statistical analysis of each beer style and finding the supra styles.

Some analyses are quite trivial and will not be detailed here. We will focus on the most interesting methods.

- What are the most common beer style? (based on number of ratings and reviews).
- Investigate if we can merge some style together. (ex: English pale ale vs American pale ale).
- What are the characteristics of each beer style (abv, location)
- What are the grades for each beer style?. (appearance, taste, palate, aroma)
- Analysis of beers style based on location.

**Task 3**: Textual analysis of the reviews in order to present a textual description of the supra-styles.

This task is essential to present a textual description of the beer styles showcased on the supermarket shelves to the beer customers in order to give them a better overview of the kind of beer they have in front of them. To do so, we try to provide, for each main beer styles, a list of adjectives that best describe the style.

To determine which adjectives best describe each style, we carry out a lexical analysis based on textual reviews. For a given beer style, the most informatives adjectives are those which occure the most in the textual reviews about the given beer style and which doesn't occure to much in the reviews of the other beer styles. To adjust for the fact that some adjectives appear more frequently in general (for example 'good' or 'bad'), we will use a TF-IDF approach, as it is one of the most popular term-weighting schemes today.

This lexical analysis can be decomposed in the following steps:

- Step 1: Group reviews by language.
- Step 2: Group reviews by beer styles.
- Step 3: Extract adjectives from the textual reviews.
- Step 4: Group adjectives by supra style.
- Step 5: Compute the TF-IDF matrix where the documents are the list of adjectives for each supra styles.
- Step 6: Visualize the most informatives adjectives per supra style.
For this lexical analysis one of the main assumption is that most of the adjectives in the reviews are actually used to describe the beer.

**Task4**: Categorisation of the users.

In this part of the analysis, the objective is to categories the users. These categories of users will be the different shelves that each supermaket will have to put in its beers aisle. The different categories that we think can be relevant for the customers are 'experienced beer consumers' which will target the customers which are familiar with beers and consider themselves as regular consumers. 'Novice beer consumers' which on the other hand are rather less accustomed to beer consumption. Another category to showcase is 'explorator beer consumers' which targets all customers which want to try a new kind of beer and want to discover something new. The analysis for this category is based on the users which tend to rate beers that are less popular. Finally, the category 'nationalist' doens't require any processing on the users but will be used to distonguish the supermarket according to their country. It will thus be used to showcase all the beers that are the most popular in the country we are in.

- Step 1: Perform a linear regression to justify the fact that the level of experience of a user and his country of origin influences his preferences in beer styles.
- Step 2: Find the thresholds for the different categories
- Step 3: Categorise the users

**Task 5**: Find the favorite supra-styles for each category of users.

Once we have our users categories, we want to determine which are the styles of beers users prefer depending on their country and level of experience. For this task we first use a Bayesian approach to estimate ratings given beer features (brewery, location, abv, beer style) and remove ratings which are too far from our prediction. We will then compute the average scores for each user category for each style and sort out the most liked ones

**Task 6**: Find the best beers to place on the shelves.

To determine which beers to place on the shelf for each category, we set a threshold to the minimum number of ratings such that we take into account the rating of this beer to find the best beers for each style. In fact, one beer might have been rated only once by a user who gave it a very high rating, but another user might have given it a lower rating, thus this beer should not necessarily be prefered over a beer with slightly lower average rating but with many ratings.

**Task 7**: Github-site building and data story redaction.


## 🤝 Team members contribution
<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky">Team member</th>
    <th class="tg-0pky">Contribution</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky">Lola Vegeas</td>
    <td class="tg-0pky">step 1 task 4,task 5&6, task 7</td>
  </tr>
  <tr>
    <td class="tg-0pky">Jean Decroux</td>
    <td class="tg-0pky">task 1, task 2, task 7</td>
  </tr>
  <tr>
    <td class="tg-0pky">Ramy Charfeddine</td>
    <td class="tg-0pky"> step 2&3 task 4, task 7, README</td>
  </tr>
  <tr>
    <td class="tg-0pky">Tom Mery</td>
    <td class="tg-0pky">task 1, task 3, task 7, put Notebooks in common</td>
  </tr>
</tbody>
</table>

## Authors
- Lola Vegeas (lola.vegeas@epfl.ch)
- Jean Decroux (jean.decroux@epfl.ch)
- Ramy Charfeddine (ramy.charfeddine@epfl.ch)
- Tom Mery (tom.mery@epfl.ch)
