# Radius data challenge
### Description of data:
1. The data has 1 million rows and 10 columns. They represent company information such as: address, categorical code, city, headcount, name, phone, revenue, time in business, and zip code.
2. As a preliminary data quality check, the missing rate for each feature are examined, here is a summary:
| Feature       | fill rate    |
| ------------- |-------------:|
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

3. The next step is to check the valid data input for each of the field, for example: some fields have empty string, invalid category code or zip code. It is important to identify valid input to ensure the data quality. Here are some rules for valid input for each field
..* address: address has to contain letter
..* category_code: it needs to be a series of number has length 8
..* city: city needs to contain letter
..* headcount: needs to contain number or letter
..* name: needs to contain letter or number
..* phone: needs to contain number
..* revenue: needs to contain number or letter
..* state: needs to contain letter
..* time_in_business: needs to contain letter or number
..* zip: needs to contain number has length at least 5
