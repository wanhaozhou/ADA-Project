# Spotting fake reviews in Amazon review data

Learning and Mining from the Amazon Data

Group Members: [Siyuan Li](mailto:siyuan.li@epfl.ch), [Yuanfei Mai](mailto:yuanfei.mai@epfl.ch), [Wanhao Zhou](mailto:wanhao.zhou@epfl.ch)

# Abstract
With the rapid growth of online reviews, it has become a common practice for people to read reviews for many purposes. This gives rise to review spam, i.e., writing fake reviews to mislead readers by giving bogus positive or negative opinions to some target products to promote them or to damage their reputations. Our task is to find those reviewers who write fake reviews. The problem can be seen as a classification problem but obtaining training data by manually labelling reviews is very hard. In this project, we hope to study the unusual behaviours of reviewers who are of suspect spam reviewers. We make the following assumptions to support our analysis. Firstly, we assume that the majority of reviewers are normal reviewers. Secondly, we assume that there exists statistical difference between spam reviewers and normal reviewers. 
We hope to address the problem from two aspects. First, we hope to choose features based on common sense to identify such reviewers and reviews. Second, we would like to use statistical models to find the useful features that can distinct the unusual patterns from majority.

# Research questions
Based on above assumptions, our task is clear: detect those highly suspicious reviewers by finding unusual statistical patterns that are distinct from normal patterns. In other ways, spotting outlier. 

Questions are as follows:

1. How to spot fake reviews on Amazon and who are suspicious reviewers that write fake reviews on Amazon.
2. Given such fake reviews and suspicious reviewers, who are the "greedy" sellers (brands) that hired those fake reviewers.
3. Based on statistical rules, give some basic tricks to spot a fake review. Apply our rules to other categories on Amazon to distinguish between fake reviews.



# Dataset
Generally, our project is based on the [Amazon Review Dataset](http://jmcauley.ucsd.edu/data/amazon/) . The dataset consists of product reviews and metadata from Amazon, including 142.8 million reviews spanning May 1996 - July 2014. 
We divide the whole data according to categories. We view these categories as two major types: **content-based product** and **brand-based product**.
- Content-based product: this refers to products like books, movies and TV, video games, etc. In this subcategory of product, people are more interested in the content of the product,. The brand itself, if any, does not matter much to the customer.
- Brand-based product: this refers to products like clothes, home appliances, etc. Customers attitudes towards such products have something to do with the brands, e.g., some customers might be loyal to certain brands.

We would like to experiment with the typical data of these two subcategories. Our project so far is based on the following data (both reviews and the product metadata):
- Movies and TV (content-based)
	- In addition, we make use of [IMDB ratings](https://datasets.imdbws.com/) to give us a relative objective rating for movies and TV.
		- IMDB is an online database of information related to films, television programs, home videos and video games, and internet streams, including cast, production crew and personnel biographies, plot summaries, trivia, and fan reviews and ratings.
		- We find that usually content-based product has a very good feature: most of them have other rating database. These ratings are created by those truly love movies or books. Their ratings are more objective than a commercial website. But there also exist bias on those datasets. But in our assumption, we consider these bias much smaller than Amazon data.
		- Based on above assumption, then we can use those relatively objective database to verify the validity of our approach to detect fake reviews, at least biased reviews. The concept of this test is: if we remove those fake reviews (detected by our analysis). The rating distribution of Amazon movies should be similar like IMDB data.
- Clothing, Shoes and Jewellery (brand-based)
- Home and Kitchen (brand based)


# Data wrangling and analysis in milestone 2
The following wrangling and analysis are applied to the dataset:
- Amazon Dataset Selection: Categories and Size
	- We chose two kinds of products that have different rules to find out the fake reviews. Basically, one category contains products that have brands, namely brands-based products. Another one contains those without brands, namely content-based products. 
	- In our research, those products and users that have only few reviews can not help us to find out the regulation of fake reviews therefore we only chose the dataset in which each of the reviewers and products have at least 5 reviews, called 5-core dataset.

- Data Initial Observation And Cleaning
	- In reviews data, there are several features that we found useful in our further study like reviewerID, asin (products ID),helpful(reviews helpfulness), overall(rate given out by reiewers), unixReviewTime, reviewTime(the time of the reviews come out). Noted that the content in 'helpful' column should be transformed into numerical values.
	- In metadata, the features like asin, brand, title seems useful. Noted that there are some missing values in brand that need further processing in 'brand' column.

- Search for missing values in brand column
	- After calculation of the portion of the NaN in brand column, brand-based products have lower portion of NaN compared with content-based products. That's why we utilize the brand features to find out the suspicious reviewers on brand-based products, but utilize overall features (overall rating) on content-based products.

- Search for impossible values : Check the range of its columns values
No impossible values in overall rates were found.

- Features Extraction: extracted the useful features we mentioned above

- Data Deeper Observation
	- Distribution of number of the reviews of the products
		- Among these three categories, most of the products have fewer than 20 reviews.
	- Distribution of number of the reviews of the users
		- Among these three categories, most of the users have fewer than 20 reviews.
	- Distribution of ratings of the item that have largest number of reviews
		- Among these three categories, the distributions of the reviews for the popular products look similar - most of the reviews gave out 5 stars rates.
	- Distribution of ratings from the users that wrote the largest number of reviews
		- Among these three categories, the distributions of the reviews from various users vary a lot from person to person.

- Data Enrichment
	- Based on the rating distribution of each product, we found out reviews whose ratings were out of the corresponding rating distribution confidence interval (80% for instance). For those out of the confidence interval range, we labeled them as 1 meaning potential suspicious candidates and 0 otherwise.


# Proposed approaches to our research questions
Typically, we hope to address the problem from two aspects. 

**First**, we hope to choose features based on common sense to identify such reviewers and reviews. This applies to **content-based product**,For example, we can choose the rating of a certain product as a feature.  A reviewer can be suspicious of malicious attack if he gives a low rating but majority gives high.  

We formulate the problem as follows: 

- Given a product's overall reviews, if a reviewer's rating deviates too much from other customers' reviews, then this review will be considered as suspicious.
- If the majority of a reviewer's reviews are considered suspicious in this sense, then this reviewer will be seen as a spam reviewer candidate.
- Also, we consider using IMDB ratings to give us a more objective rating of the products. This also helps us to identify such reviews and reviewers based on the rules above.
- How to measure the deviation and the majority to which content need us to find out with close inspection of data.

**Second**, we would like to use statistical models to find the useful features that can distinct the unusual patterns from majority. This applies to **brand-based product**, since ratings like IMDB are usually absent and we would like to make use of the rich information of the brand.

We formulate the problem as follows[1]:
- Given a review data record, which contains attributes (e.g. reviewer id, brand, product id, etc.), denoted as ![A_j](https://latex.codecogs.com/gif.latex?A_j) and a rating for the product (![c_i](https://latex.codecogs.com/gif.latex?c_i)), we would like to find the statistical pattern under the data.
- We hypothesise first that the attributes and the rating are independent, i.e. ![P(c_i) = P(c_i|A_j)](https://latex.codecogs.com/gif.latex?P%28c_i%29%20=%20P%28c_i%7CA_j%29). Then we calculate the **confidence unexpectedness (cu)** and **support unexpectedness (su)** for each rating class.
	- confidence unexpectedness: let's say ![$v_{jk}$](https://latex.codecogs.com/gif.latex?v_%7Bjk%7D) is a possible value of the attribute ![$j$](https://latex.codecogs.com/gif.latex?j): ![$A_j$](https://latex.codecogs.com/gif.latex?A_j), the cu is defined as how ![$v_{jk}$](https://latex.codecogs.com/gif.latex?v_%7Bjk%7D) leads to a possible rating class ![$c_i$](https://latex.codecogs.com/gif.latex?c_i), 
i.e. ![$$cu(v_{jk} \to c_i) = \frac{\mathbb{P}(c_i|v_{jk}) - \mathbb{E}_k\[\mathbb{P}(c_i|v_{jk})\]}{\mathbb{E}_k\[\mathbb{P}(c_i|v_{jk})\]}$$](https://latex.codecogs.com/gif.latex?$$cu%28v_%7Bjk%7D%20%5Cto%20c_i%29%20=%20%5Cfrac%7B%5Cmathbb%7BP%7D%28c_i%7Cv_%7Bjk%7D%29%20-%20%5Cmathbb%7BE%7D_k%5B%5Cmathbb%7BP%7D%28c_i%7Cv_%7Bjk%7D%29%5D%7D%7B%5Cmathbb%7BE%7D_k%5B%5Cmathbb%7BP%7D%28c_i%7Cv_%7Bjk%7D%29%5D%7D$$)
where under the aforementioned independence hypothesis, ![$$\mathbb{E}_k\[\mathbb{P}(c_i|v_{jk})\] = \mathbb{P}(c_i)$$](https://latex.codecogs.com/gif.latex?$$%5Cmathbb%7BE%7D_k%5B%5Cmathbb%7BP%7D%28c_i%7Cv_%7Bjk%7D%29%5D%20=%20%5Cmathbb%7BP%7D%28c_i%29$$)
Intuitively, this rule could be used to suspect users who give high/low ratings on a particular brand.
	- support unexpectedness: similarly, su considers the unexpectedness in joint probability. 
![$$su(v_{jk} \to c_i) = \frac{\mathbb{P}(c_i, v_{jk}) - \mathbb{E}_k\[\mathbb{P}(c_i, v_{jk})\]}{\mathbb{E}_k\[\mathbb{P}(c_i, v_{jk})\]}$$](https://latex.codecogs.com/gif.latex?$$su%28v_%7Bjk%7D%20%5Cto%20c_i%29%20=%20%5Cfrac%7B%5Cmathbb%7BP%7D%28c_i,%20v_%7Bjk%7D%29%20-%20%5Cmathbb%7BE%7D_k%5B%5Cmathbb%7BP%7D%28c_i,%20v_%7Bjk%7D%29%5D%7D%7B%5Cmathbb%7BE%7D_k%5B%5Cmathbb%7BP%7D%28c_i,%20v_%7Bjk%7D%29%5D%7D$$) 
where ![$$\mathbb{E}_k\[\mathbb{P}(c_i, v_{jk})\] = \mathbb{P}(c_i)\frac{\displaystyle\sum_{l=1}^{|A_j|} \mathbb{P}(v_{jl})}{|A_j|}$$](https://latex.codecogs.com/gif.latex?$$%5Cmathbb%7BE%7D_k%5B%5Cmathbb%7BP%7D%28c_i,%20v_%7Bjk%7D%29%5D%20=%20%5Cmathbb%7BP%7D%28c_i%29%5Cfrac%7B%5Cdisplaystyle%5Csum_%7Bl=1%7D%5E%7B%7CA_j%7C%7D%20%5Cmathbb%7BP%7D%28v_%7Bjl%7D%29%7D%7B%7CA_j%7C%7D$$) Intuitively, this rule could be used to suspect users whose reviews concentrate on a small range of brands.


[1]: We follow the ideas in this paper: https://www.cs.uic.edu/~liub/publications/CIKM-final-unexpected.pdf



# Questions for TA
~~1. Would you please offer us any advice on how to identify a product's quality regardless of the cover, i.e. the quality itself of the product, not the cover? Are there any datasets describing this attribute?~~

~~2. Since this is a primitive plan of the project, can we change the steps or add the data that is not included in this report in the course of the project?~~

~~3. Marked in bold in the dataset section described above, can we put the missing (but maybe useful) data on the cluster for later use?~~
