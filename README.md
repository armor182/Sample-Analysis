# Amazon Project
## Introduction
The broad topic that drives my attention is consumers' purchasing behaviors and patterns on e-commerce platforms. Therefore, I chose Amazon data as it provides rich information about product categories, product timing, and consumer demographics, and I would like to dive deeper into the specific consumer data. After the initial exploration of the data, I found one product I am particularly interested in, which is the category of gift cards. Focusing on gift card categories, there are four questions I am targeting for:

When do consumers most frequently purchase gift cards, and is there a discernible time trend?
What are the demographic patterns associated with gift card purchases?
How often, how recently, and how much do consumers spend when purchasing gift cards?
Do customers typically purchase gift cards together with other product categories, or do they usually buy them on their own?


## Data Preparation
### Datasets
The datasets used for this project is drawn from the Amazon Purchase and Life Changes Survey Dataset available on Harvard Dataverse. The dataset contains respondent' demographic information and Amazon spending behavior. Each observation represents an individual survey respondent with associated spending data. 

I will be using two datasets from the original study: the `amazon-purchase.csv` and the `survey.csv`. The transactional purchase dataset includes details of Survey ResponseID, order date, shipping state, unit price, quantity, product code, title, and category. The survey dataset provides corresponding demographic and spending habit information, also with the Survey ResponseID column. 

Primary Source: Amazon Purchase and Life Changes Dataset (Harvard Dataverse, 2021)
https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/YGLYDY

### Data Cleaning
The two datasets were joined to create a unified analytical dataset for our project, using the Survey ResponseID as the primary key. This merge combines individual purchase records with the respondent's demographic profile, allowing for a complete look at purchasing behavior. 

Data filtering was done to focus on spending behaviors associated with gift cards. It was done through Category filtering, ensuring only records related to the purchases of **gift cards** and associated products are retained. 

Moreover, the total price per order was calculated by multiplying the purchase price per unit by the quantity. Then, the top 99th percentile of gift card purchases were also filtered out because they created extra spikes in trend analysis. Several records with unusually small purchase amounts, likely representing system-generated reloads or potential fraudulent transactions rather than genuine purchases, were filtered out to improve data reliability and ensure the analysis reflects meaningful consumer behavior. 

The entire cleaning process was implemented in Python. Please refer to the attached `Data_PreCleaning.ipynb` document for a detailed description of the cleaning process. 


## Data Analysis
### Time Series Analysis
The most recent transaction in the dataset is dated March 20, 2023, indicating that the 2023 data is incomplete for any standardized temporal analysis (e.g., by year, quarter, or month). To maintain a consistent and unbiased time series visualizations, all 2023 records were excluded, limiting the analysis to the complete calendar years from 2018 to 2022. 
#### Giftcard Sales Trend by Year
<img width="800" height="500" alt="Gitcard Sales Trend by Year" src="https://github.com/user-attachments/assets/8e3a26d5-de95-46a5-a5d7-9327caa64302" />

This chart illustrates the trend of gift card sales quantity from 2018 to 2022. From 2018 to 2020, there was a sharp increase in the quantity purchased by the customers. Then, after 2020, there is a short decrease, and in 2021, the quantity decreases sharply. This downward trend suggests that the pandemic may have reduced consumers' use of gift cards, possibly due to economic uncertainty or a decrease in social gatherings that limited the need for gifting.

#### Quarterly Giftcard Sales by Year
<img width="800" height="500" alt="Quarterly Trend" src="https://github.com/user-attachments/assets/84ccc71d-5334-4a3e-aa3f-a345387ab40a" />

Those charts illustrated the quarterly trends in gift card sales quantity from 2018 to 2022. Throughout those five years, there is a consistent spike in gift card sales quantity in Q4, which could be driven by the holiday season, such as Black Friday, or end-of-year gifting activities. Additionally, from 2020 to 2021, we can also see a spike during Q2, which could potentially be due to the graduation session, during which students need to exchange gift cards. Those findings demonstrated that certain key social events and seasonal events can lead to a sharp increase in gift card sales throughout the year. 

#### Monthly Distribution of Gift Card Sales
<img width="800" height="500" alt="Monthly Distribution" src="https://github.com/user-attachments/assets/42e02433-d85e-4804-ac32-a42415ef4099" />

This monthly trend demonstrated that June (Graduation Season) and December (holiday shopping periods) were the two spikes in gift card sales, which further validates that social events, such as Graduation Seasons, and seasonal sales, such as Black Friday, can drive gift card sales.


### Regional Pattern
#### Giftcard Purchases by State
<img width="800" height="494" alt="newplot" src="https://github.com/user-attachments/assets/94e0c99b-ec4e-4950-8a47-00d2b5b57b0c" />

This map illustrates the geographic distribution of gift cards across U.S. states, with darker colors indicating lower quantities of sales and lighter colors indicating higher quantities of sales. From this map, we can see that California is the state with the highest number of gift card sales. Then, Texas, Florida, and NewYork also have relatively high sales. Then, states in the Midwest have relatively low sales. Such a difference demonstrated that population size and urbanization can be important factors influencing gift card quantity sales. 


### Gender Pattern
#### Distribution of Purchase Quantity by Gender
<img width="800" height="500" alt="gender" src="https://github.com/user-attachments/assets/b47306b9-b0b1-4877-805b-86c3e781e331" />

This chart illustrates the distribution of gift card sales by gender. The chart shows that male customers purchased more gift cards than female customers. This difference can possibly be explained by shopping differences across different genders (Business Settings or Personal Gifting). The difference between the two genders is not extremely large, suggesting that both genders contribute to the quantity of gift card sales. Therefore, it is relatively difficult to conclude which gender is more likely or the main contributor based on our limited data.


### Age Pattern
#### Purchase Quantity Distribution by Age
<img width="800" height="500" alt="Age" src="https://github.com/user-attachments/assets/a7f7142a-19f5-4773-add4-aa1ec822bce3" />

The bar chart illustrates the total quantity of gift cards purchased across different age groups. The data reveal that individuals aged 25–34 and 35–44 are the dominant buyers, accounting for the largest purchase volumes at 16,142 and 13,373 units respectively, suggesting that mid-career adults are the primary consumers of gift cards. Purchases then decline sharply among older cohorts. This pattern implies that gift card purchasing behavior on Amazon is most prevalent among younger to middle-aged adults, likely reflecting higher digital familiarity and gifting activity within these demographics. 


### Retention Analysis
Then, to answer the research question "How often, how recently, and how much do consumers spend when purchasing gift cards?", we introduced the RFM model to answer this question. RFM model is a marketing technique used to segment consumers based on their purchasing behaviors. There are three evaluation metrics in the model:

1. Recency (how recently the consumer bought)
2. Frequency (how often the customer buys)
3. Monetary (How much the customer spends)

By examining these three metrics, we can gain insight into a customer's purchasing behavior, including their most recent orders and the total amount spent on the platform. This can help indicate customer segmentation on the platform and provide insights into consumer loyalty and spending patterns within the gift card category.

Here, we used 'filt_price' to avovid potential fraud purcahses that might influence analysis on customers' retention. 

#### Frequency VS Recency Plot
<img width="800" height="500" alt="1" src="https://github.com/user-attachments/assets/47e882af-7fc5-41c9-af51-93057da6b40b" />

This scatter plot demonstrated the relationship between Recency and Frequency among customers. We can see that most customers cluster near the bottom-left corner, indicating that they make only a few purchases and do so within a short time period. From the platform's perspective, we can categorize them as occasional buyers. Then there is a small group of customers that stands out with very high purchase frequencies and low recency values, meaning they are the very loyal customers who contribute significantly to the overall sales. 

#### Recency VS Monetary
<img width="800" height="500" alt="2" src="https://github.com/user-attachments/assets/4165aa38-9379-4818-b34a-a369ec409d1c" />

The scatter plot illustrates the relationship between recency (days since last purchase) and monetary value (total spending) among gift card buyers. The pattern shows a clear negative association: customers who purchased more recently tend to have higher total spending, while those with longer gaps since their last purchase generally spend less. Most data points cluster near low recency and low-to-moderate spending, suggesting that recent and moderately active customers contribute more to gift card sales, where as infrequent or inactive buyers exhibit less monetary engagement. 

#### Frequency VS Monetary
<img width="800" height="500" alt="3" src="https://github.com/user-attachments/assets/959331bd-d7e1-4f36-a6a7-8869db8465dc" />

The scatter plot depicts the relationship between purchase frequency (number of transactions) and monetary value (total spending) among gift card buyers. Most customers cluster near the lower left corner, indicating low purchase frequency and modest total spending, suggesting that the majority are occasional buyers. However, a small number of outliers exhibit both high spending and high frequency, representing a niche group of loyal and heavy purchasers. Overall, the distribution highlights a highly skewed purchasing pattern, where frequent and high-value buyers are rare but may disproportionately contribute to total sales.

#### RFM Heatmap
<img width="800" height="500" alt="4" src="https://github.com/user-attachments/assets/69e1c023-8ddc-44ce-a2ca-1c92d7ac0990" />

The heatmap visualized the score for the three evaluation metrics. The upper-right red area showed that customers with both high frequency and relatively recent purchases exhibit the highest average spending, showing that they are the platform's most valuable and loyal customers. In contrast, customers with low frequency and longer gaps sicne their last purchase contribute far less, as indicated by the light-colored regions. Therefore, we can conclude that frequent and recently active customers are key drivers of sales, demonstrating the importance of incorporating retention strategies that encourage repeat purchases and ongoing engagement over time. 


### Combo Analysis
In this section, we investigate whether gift cards tend to be purchased on their own or alongside other related products. Specifically, we focus on gift wrap and gift envelopes, which are commonly paired with gift cards as part of a gifting bundle.

Because our dataset spans multiple years and customers may purchase items in separate transactions, it is difficult to determine whether products were bought together. To address this, we analyze purchase behavior at the year level: if a customer purchased both a gift card and either gift wrap or a gift envelope within the same year, we classify this as a combo purchase.

#### Distribution of Combo Analysis
<img width="800" height="500" alt="5" src="https://github.com/user-attachments/assets/696ab429-a4f2-40d9-ad2a-080e9f6d85f8" />


## Conclusion
This project investigates gift card purchase behavior on Amazon by examining demographic patterns, seasonal trends, and individual purchasing characteristics such as frequency, total spending, recency, and cross-category purchasing.

The findings show that gift cards are most commonly purchased by customers in California, male users, and the middle-age demographic group. In terms of seasonality, purchases peak in Q4, particularly in December, which aligns with the holiday gifting season. I also observe notable increases in gift card purchases during 2020–2021, likely influenced by the COVID-19 pandemic, during which people were more inclined to shop online, and sending digital gift cards became a practical way to maintain social connection while social distancing.
At the individual level, customers who purchased more recently also tended to purchase more frequently and spend more overall, and higher purchase frequency was associated with higher total monetary value. In other words, I find that customers who purchased more recently are more likely to be repeat buyers, and repeat buyers tend to be the highest spenders. This pattern highlights an opportunity for targeted retention strategies. Specifically, sending timely gifting reminders or personalized prompts to recent and frequent buyers could effectively encourage continued purchasing and reinforce their existing buying behavior. In terms of combo analysis, my findings show that gift cards are most often purchased on their own, rather than as part of a larger gifting bundle with items such as gift wrap or envelopes.

Overall, my analysis provides insights into gift card purchasing patterns across demographic groups, seasonal trends, and individual buying behaviors. These findings can help inform sales strategies such as regional targeting, seasonal campaign planning, and personalized promotional messaging for gift cards on Amazon.

Several limitations remain in my analysis. First, I implicitly assume that gift card purchases are primarily intended for gifting. In reality, customers may also purchase or reload gift cards for personal use, such as managing spending or taking advantage of account rewards. This could explain why gift cards frequently appear less in combination with other gifting-related items. Future analysis could address this by distinguishing gift card reloads from new gift card purchases to better understand the underlying purchase intent. Second, even when I evaluate combo purchases at the year level, I cannot determine whether gift cards and related items were purchased for the same occasion or for different purposes. As a result, my combo measure may overestimate or underestimate true gifting bundles. Collecting more granular contextual or transaction-level intent data would allow for a more accurate analysis of gifting behavior.
