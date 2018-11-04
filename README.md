# Unveiling the Covers' Impacts on Media Products

Learning and Mining from the Amazon Data

Group Members: [Siyuan Li](mailto:siyuan.li@epfl.ch), [Yuanfei Mai](mailto:yuanfei.mai@epfl.ch), [Wanhao Zhou](mailto:wanhao.zhou@epfl.ch)

# Abstract
Here is a well-known motto -- *don't judge a book by its cover*. However, human does like pretty things, which seems paradoxical to the motto. Which is the truth in reality? In this project, the Amazon dataset is a nice material to back up our research since it contains the data of the cover features and crucial comments. We limit our research scope to the following products: books, movies (including TV), music and video games, namely media products. We are going to analyse the relations between the cover features and their sales, reviews and ratings. We hope to draw some conclusions based on our analysis, which might be helpful to producers and customers.

# Research questions
1. Does a media product's cover impact its ratings/reviews? If so, what visual features are important to this impact? How to generally describe the impact (are there any general patterns)? Does this impact evolve with the time? Is there any differences among subcategories of these products?
2. What about the cover's impact on sales? Does it follow the pattern discovered before? Are there any new findings?
3. Based on the the analysis above, what advice can be given to producers and customers when designing and choosing products? Furthermore, what predictions can we derive from the analysis?

# Dataset
Generally, our project is based on the [Amazon Review Dataset](http://jmcauley.ucsd.edu/data/amazon/) available on the cluster. The dataset consists of product reviews and metadata from Amazon, including 142.8 million reviews spanning May 1996 - July 2014. We are particularly interested in the following subcategories, **Books, Movies and TV,  Digital Music and Video Games**, which we refer to as media products. The dataset includes three parts: reviews (ratings, text, helpfulness votes), product metadata (descriptions, category information, price, brand, and image features), and visual features (which are extracted from each product image using a deep CNN, **not on cluster already**).

Also, we are considering a complementary dataset to the Amazon Review Dataset, called [Amazon Question and Answer Data](https://cseweb.ucsd.edu/~jmcauley/datasets.html#amazon_qa) (**not on cluster already**) containing questions and answers about products from the Amazon dataset, which might hopefully offer alternative views from customers.


# A list of internal milestones up until project milestone 2
Before the milestone 2, we first need to have a clean and useful dataset. Additionally, the blueprint for the analysis steps should be drafted.

Jobs related to the dataset: 
1. Obtain the dataset(s) we need described in the section above.
2. Examine the dataset(s), and read relevant papers to figure out which subset of data are useful.
3. Extract the useful data, clean the redundant information and format the data based on the goals.
4. For visual features of the data, examine the utility of the current version. Extract alternative visual features from original images using other neural networks if needed.

Jobs related to the analysis:
 1. Read relevant papers of tasks performed on the dataset(s) to get familiar with the dataset(s)
 2. Formulate methods to model the users' behaviour/attitude
    based on the reviews (maybe including answers).
 3. Consistently check what type of data is necessary for the corresponding investigated relation.
 4. Analyse the pattern along the time period and within different subcategories.

# Questions for TAa
1. Would you please offer us any advice on how to identify a product's quality regardless of the cover, i.e. the quality itself of the product, not the cover? Are there any datasets describing this attribute?
2. Since this is a primitive plan of the project, can we change the steps or add the data that out of this report in the course of the project?
3. Marked in bold in the dataset section described above, can we put the missing (but maybe useful) data on the cluster for later use?
