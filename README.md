# Spotting fake reviews in Amazon review data

Learning and Mining from the Amazon Data

Group Members: [Siyuan Li](mailto:siyuan.li@epfl.ch), [Yuanfei Mai](mailto:yuanfei.mai@epfl.ch), [Wanhao Zhou](mailto:wanhao.zhou@epfl.ch)

# Abstract
With the rapid growth of online reviews, it has become a common practice for people to read reviews for many purposes. This gives rise to review spam, i.e., writing fake reviews to mislead readers by giving bogus positive or negative opinions to some target products to promote them or to damage their reputations.  The problem can be seen as a classification problem but obtaining training data by manually labelling reviews is very hard.  In this project, we hope to study the unusual behaviour of reviewers who are of suspect spam reviewers. Typically, we hope to address the problem from two aspects. **First**, we hope to apply human-crafted rules based on common sense to identify such reviewers and reviews. **Second**, we would like to use statistical models that could generalise the problem and might be applied to a broader scope.

# Research questions
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
- Clothing, Shoes and Jewellery (brand-based)
- Home and Kitchen (brand based)

# Data wrangling and analysis in milestone 2
**First**, to make our data concise and much more useful, we only keep the **5-core** version of our data. 5-core data means that it is a subset of the data in which all users and items have at least 5 reviews.
-   That you understand what’s into the data (formats, distributions, missing values, correlations, etc.).
-   That you considered ways to enrich, filter, transform the data according to your needs.
-   That you have updated your plan in a reasonable way, reflecting your improved knowledge after data acquaintance. In particular, discuss how your data suits your project needs and discuss the methods you’re going to use, giving their essential mathematical details in the notebook.
-   That your plan for analysis and communication is now reasonable and sound, potentially discussing alternatives to your choices that you considered but dropped.

# Proposed approaches to our research questions
**First**, we hope to apply human-crafted rules based on common sense to identify such reviewers and reviews. This applies to **content-based product**: movies and TV. 

We formulate the problem as follows: 

- Given a product's overall reviews, if a reviewer's rating deviates too much from other customers' reviews, then this review will be considered as suspicious.
- If the majority of a reviewer's reviews are considered suspicious in this sense, then this reviewer will be seen as a spam reviewer candidate.
- Also, we consider using IMDB ratings to give us a more objective rating of the products. This also helps us to identify such reviews and reviewers based on the rules above.
- How to measure the deviation and the majority to which content need us to find out with close inspection of data.

**Second**, we would like to use statistical models that could generalise the problem (and might be applied to a broader scope). This applies to **brand-based product**, since ratings like IMDB are usually absent and we would like to make use of the rich information of the brand.

We formulate the problem as follows[^1]:
- Given a review data record, which contains attributes (e.g. reviewer id, brand, product id, etc.), denoted as $A_j$ and a rating for the product ($c_i$), we would like to find the statistical pattern under the data.
- We hypothesise first that the attributes and the rating are independent, i.e. $P(c_i) = P(c_i|A_j)$. Then we calculate the **confidence unexpectedness (cu)** and **support unexpectedness (su)** for each rating class.
	- confidence unexpectedness: let's say $v_{jk}$ is a possible value of the attribute $j$: $A_j$, the cu is defined as how $v_{jk}$ leads to a possible rating class $c_i$, i.e. $$cu(v_{jk} \to c_i) = \frac{\mathbb{P}(c_i|v_{jk}) - \mathbb{E}_k[\mathbb{P}(c_i|v_{jk})]}{\mathbb{E}_k[\mathbb{P}(c_i|v_{jk})]}$$where under the aforementioned independence hypothesis, $$\mathbb{E}_k[\mathbb{P}(c_i|v_{jk})] = \mathbb{P}(c_i)$$
Intuitively, this rule could be used to suspect users who give high/low ratings on a particular band.
	- support unexpectedness: similarly, su considers the unexpectedness in joint probability. $$su(v_{jk} \to c_i) = \frac{\mathbb{P}(c_i, v_{jk}) - \mathbb{E}_k[\mathbb{P}(c_i, v_{jk})]}{\mathbb{E}_k[\mathbb{P}(c_i, v_{jk})]}$$ where $$\mathbb{E}_k[\mathbb{P}(c_i, v_{jk})] = \mathbb{P}(c_i)\frac{\displaystyle\sum_{l=1}^{|A_j|} \mathbb{P}(v_{jl})}{|A_j|}$$ Intuitively, this rule could be used to suspect users whose reviews concentrate on a small range of bands.


[^1]: We follow the ideas in this paper: https://www.cs.uic.edu/~liub/publications/CIKM-final-unexpected.pdf



# A list of internal milestones up until project milestone 3
Before the milestone 3, 

# Questions for TA
~~1. Would you please offer us any advice on how to identify a product's quality regardless of the cover, i.e. the quality itself of the product, not the cover? Are there any datasets describing this attribute?~~
~~2. Since this is a primitive plan of the project, can we change the steps or add the data that is not included in this report in the course of the project?~~
~~3. Marked in bold in the dataset section described above, can we put the missing (but maybe useful) data on the cluster for later use?~~
