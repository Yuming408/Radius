# Radius data challenge
1. Description of data:
The data has 1 million rows and 10 columns. They represent company information such as: address, categorical code, city, headcount, name, phone, revenue, time in business, and zip code.

2. As a preliminary data quality check, the missing rate for each feature are examined, here is a summary:
Most of the features less than 10% of missing rate, only the phone number has a high missing rate:  about 40% of phone number is missing.

| Feature       | fill rate    |
| ------------- |-------------|
| address       | > 99%|
|category_code  | > 99%|
|city           | > 99%|
|headcount      |96%|
|name           |> 99%|
|phone          |59%|  
|revenue        |94%|
|state          |> 99%|
|time_in_business |92%|
|zip               | > 99%|

3. The next step is to check the valid data input for each of the field, for example: some fields have empty string, invalid category code or zip code. It is important to identify valid input to ensure the data quality. Here are some rules for valid input for each field and the summary:

* address: address has to contain letter
* category_code: it needs to be a series of number has length 8
* city: city needs to contain letter
* headcount: needs to contain number or letter
* name: needs to contain letter or number
* phone: needs to contain number and has length at least 10
* revenue: needs to contain number or letter
* state: needs to contain letter
* time_in_business: needs to contain letter or number
* zip: needs to contain number has length at least 5

| Feature       | valid rate  |
| ------------- |-------------|
| address       | > 99%|
|category_code  | > 99%|
|city           | > 99%|
|headcount      |96%|
|name           |> 99%|
|phone          |59%|  
|revenue        |94%|
|state          |> 99%|
|time_in_business |92%|
|zip               | 95%|

4. After data validation, the next step is to check the cardinality in each field. Here is the summary

| Feature       | cardinality |
| ------------- |-------------|
| address       | 890k|
|category_code  | 1200|
|city           | 14k|
|headcount      |12|
|name           |890k|
|phone          |575k|  
|revenue        |14|
|state          |55|
|time_in_business |8|
|zip               |24k |

5. Observation:
* **Duplicate**:
There is some interesting observation about this data set. There is duplicate address(~108k) and phone number(~16k). For duplicate phone numbers, they seem come from different businesses in different location, possible reasons are some business changed their phone number but was not correctly reported. Further information is needed to validate the phone number. For duplicate address, they are corresponding to different business, it could be different business share the same office space.

Here are some examples:

|address|	category_code|	city|	headcount|	name|	phone|	revenue|	state|	time_in_business|	zip|
|-------|--------------|------|----------|------|------|---------|-------|------------------|----|
|656 AVONMORE DR|	52200000|	FORT MILL|20 to 49|	Kusch's Automotive|	**(703) 234-4646**|	Less Than $500,000|SC|10+ years|29715|
|49 KIHAPAI ST|42490000|KAILUA|	50 to 99|	Mohawk Lifts Of Pa Inc|	**(703) 234-4646**|	$5 to 10 Million|	HI|	6-10 years|96734|
|**016 KEARNS RD**|	62130000|	SNOWMASS VILLAGE|	< 50|	Billings Fire Department|	9787745887|	Less Than $500,000|CO|10+ years|	81615|
|**016 KEARNS RD**|	81000000|	SNOWMASS VILLAGE|	< 50|	Symes Saab|	None|	$500,000to 1 Million|	CO|10+ years|81615|	

* **Missing data**
There are only 59% of the phone numbers are valid. To examine the reason,  fill rate of phone number group by headcount, revenue, state, and time_in_business were explored. The fill rate is about 58-59% for each of the group, so it seems the missing phone number is randomly distributed
* **Business size**
I am interested in segment business size according to the headcount. Since most of the Radius's customers are small to medium size of business, so let's focus on insights of small businesses. I choose less than 250 headcounts as the group for the analysis

|headcount| # of business|
|----------|--------------|
|< 50|           758k|
|50 to 250|       93k|
|250 to 1000|     16k|
|> 1000 |          5.6k|

* **Business profile: revenue versus years in business**
I am interested in looking at the pivot table of revenue versus years in business. We want to understand what are the small business profiles look like. From the table, we can see for small business, the majorities are having business more than 10 years and revenue are less than 2.5 million, so there is some potential there to help them grow their business

|time_in_business|	1-2 years|	3-5 years|	6-10 years|	10+ years|
|----------------|--------|----------|--------|---------|
|Less Than $500,000|	1%|	1%|	4%|	28%|
|$500,000 to 1 Million|	< 1%|	1%|	2%|	13%|
|$1 to 2.5 Million|	< 1%|	1%|	2%|	15%|
|$2.5 to 5 Million|	< 1%|	< 1%|	1%|	9%|
|$5 to 10 Million|	< 1%|	< 1%|	1%|	7%|
|$10 to 20 Million|	< 1%|	 < 1%|	1%|	4%|
|$20 to 50 Million|	< 1%|	< 1%|	< 1%|	3%|
|$50 to 100 Million| < 1%|< 1%|	< 1%|1%|
|$100 to 500 Million|	< 1%|	< 1%|	< 1%|1%|
|Over $500 Million|	< 1%|	< 1%|	< 1%|	< 1%|
|Over $1 Billion|	< 1%|	< 1%|	< 1%|	< 1%|

* **Revenue in each industry sector**:
Small business owners might be curious to know where they are in the market place compare to their competitor. Next I am interested to see the revenue distribution for each sectors of industry. Two heatmap are used to visualize these information using both the absolute number of business in each cell and percent of business in each industry sector.

   Industry sector can be represented as the first two digits of nacis code

   Most of the business are in the sector of 44-45, 54, and 62, which are retail trade (44-45), Professional, Scientific,  and Technical Services (54), Health Care and Social Assistance(62), other services(81)

   About 35% of business in each industry sector, their revenue is less than 500k. It is worth to look at the percentile in each industry sector, more information about the revenue as a number is needed to compute the percentile.

   Nacis code can be found in the website: https://www.naics.com/search/
   
![alt_text](https://github.com/Yuming408/Radius/blob/master/Screen%20Shot%202017-10-10%20at%208.46.36%20PM.png "another pic")

![alt text](https://github.com/Yuming408/Radius/blob/master/Screen%20Shot%202017-10-10%20at%205.48.51%20PM.png "Industry versus revenue")

* **Industry distribution in each state**:
We want to know what is the industry components in each state, do the industry components vary a lot by state or they are similar. A heatmap is presented here to provide some insights, As we can see, the industry components seem to have similar distribution for all of the state. The top 4 industries are: Professional, Scientific, and Technical Services(54), Health Care and Social Assistance(62), retail and trade (44-45), and other services(81)

![alt_text](https://github.com/Yuming408/Radius/blob/master/Screen%20Shot%202017-10-11%20at%209.21.11%20AM.png "revenue by state")


