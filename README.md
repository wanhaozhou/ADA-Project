# Spotting Spam Reviewers in Amazon Review Data

Group Members: [Siyuan Li](mailto:siyuan.li@epfl.ch), [Yuanfei Mai](mailto:yuanfei.mai@epfl.ch), [Wanhao Zhou](mailto:wanhao.zhou@epfl.ch)

# Abstract

With the rapid growth of online reviews, it has become a common practice for people to read reviews for many purposes. This gives rise to review spam, i.e., writing fake reviews to mislead readers by giving bogus positive or negative opinions to some target products to promote them or to damage their reputations. **Our task is to find those reviewers who write fake reviews.** The problem can be seen as a classification problem but obtaining training data by manually labeling reviews is hard. In this project, we hope to study the unusual behaviors of reviewers who are of suspect spam reviewers. 

We make the following assumptions to support our analysis. First, we assume that the majority of reviewers are normal reviewers. Second, we assume that there exists statistical difference between spam reviewers and normal reviewers.

We hope to address the problem in two ways. First, we would like to use statistical models to find the useful features that can distinguish the unusual patterns of spam reviewers from the majority. Second, we are going to define features based on abnormal behaviors of spammers to identify fake reviewers and fake reviews by unsupervised learning.

# Research questions

The major problem in our project is: How to spot fake reviews on Amazon and who are suspicious reviewers that write fake reviews on Amazon in unsupervised manner?

To approach the problem, we consider the following questions:
1.  What is the rating pattern of spam reviewers and how does this pattern deviates from that of the normal users?
2.  What  features could be designed to tell spam users from normal users apart?

# Dataset

Generally, our project is based on the [Amazon Review Dataset](http://jmcauley.ucsd.edu/data/amazon/) . The dataset consists of product reviews and metadata from Amazon, including 142.8 million reviews spanning May 1996 - July 2014.

In order to avoid tons of workload, we would like to experiment with the selected category data as follows (contains both reviews and the product metadata):

- Home and Kitchen: 
	- Review data: 5-core, 551,682 reviews
	- Metadata: 436,988 products

For practical use, we use the 5-core data, which is a subset of the data in which all users and items have at least 5 reviews.

Review data:
- reviewerID  - ID of the reviewer, e.g.  A2SUAM1J3GNN3B
-   asin  - ID of the product, e.g.  0000013714
-   reviewerName  - name of the reviewer
-   helpful  - helpfulness rating of the review, e.g. 2/3
-   reviewText  - text of the review
-   overall  - rating of the product
-   summary  - summary of the review
-   unixReviewTime  - time of the review (unix time)
-   reviewTime  - time of the review (raw)

Product metadata:
-   asin  - ID of the product, e.g.  0000031852
-   title  - name of the product
-   price  - price in US dollars (at time of crawl)
-   imUrl  - url of the product image
-   related  - related products (also bought, also viewed, bought together, buy after viewing)
-   salesRank  - sales rank information
-   brand  - brand name
-   categories  - list of categories the product belongs to

# Methodology

Based on the above assumptions, our task comprises of several parts:

**First**, we detect those highly suspicious reviewers by finding unusual statistical patterns that could be distinguished from normal patterns. For example, a reviewer can be suspicious of malicious attack if he gives a low rating but majority gives high.

We formulate the problem as follows:

- Given a product's overall reviews, if a reviewer's rating deviates too much from other customers' reviews, then this reviewer will be considered as suspicious.
- The extent of deviation is measured by conditional probability and joint probability. Detailed mathematical formulas are included in the report.
- Rank the reviewers according to the deviation. The reviewers who are in the top rank are in highly suspicious.

**Second**, we would like to exploit more intrinsic features in the data and apply the unsupervised clustering algorithm to classify between spam and normal reviewers, as well as real and fake reviews . 
We first utilize the behaviors of potential spam reviewers detected by statistical rules in the first part, and design the corresponding naive features. Additionally, we manage to incorporate other features described in [1].

[1]: [Spotting Opinion Spammers using Behavioral Footprints](https://www.cs.uic.edu/~liub/publications/KDD-2013-Arjun-spam.pdf

We formulate the problem as follows:

- We design the following features. Detailed mathematical formulas and explanations are included in the report.
	- reviewers features: 
		- Review Text Similarity
		- Maximal Number of Reviews Per Day
		- Review Interval
		- First Reviewer Frequency
	- reviews features:
		- Review Repetition
		- Rating Bias
		- Extreme Rating
		- Early Post
- We utilize the K-means and Gaussian Mixture Model  cluster reviewers and reviews.

# Contributions of group members:

Siyuan Li: coming up with algorithm; coding up the first method; results of method1 analysis

Wanhao Zhou: coming up with algorithm; coding up the second method; results of method2 analysis

Yuanfei Mai: writing up the report; detailing the readme content; presenting the poster
