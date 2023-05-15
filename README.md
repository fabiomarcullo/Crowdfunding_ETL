# Project 2 - Fabio Lima & Joseph Arambulo

## Background

For the ETL mini project, you will work with a partner to practice building an ETL pipeline using Python, Pandas, and either Python dictionary methods or regular expressions to extract and transform the data. After you transform the data, you'll create four CSV files and use the CSV file data to create an ERD and a table schema. Finally, you’ll upload the CSV file data into a Postgres database.

Since this is a one-week project, make sure that you have done at least half of your project before the third day of class to stay on track.

Although you and your partner will divide the work, it’s essential to collaborate and communicate while working on different parts of the project. Be sure to check in with your partner regularly and offer support.

## Data Source

1. [Contacts Data Source](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/Resources/contacts.xlsx)

2. [Crowdfunding Data Source](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/Resources/crowdfunding.xlsx)

## Jupyter Notebook

- [ETL_Mini_Project_FLima_JArambulo](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/ETL_Mini_Project_FLima_JArambulo.ipynb)

## Output

- [Category Data Frame](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/Resources/category.csv)
- [Subcategory Data Frame](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/Resources/subcategory.csv)
- [Campaign Data Frame](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/Resources/campaign.csv)
- [Contacts Data Frame](https://github.com/fabiomarcullo/Crowdfunding_ETL/blob/main/Resources/contacts.csv)

## Project Deliverables

  ### Deliverable 1: Category and Subcategory DataFrame.

 ```python

# Read the data into a Pandas DataFrame
 crowdfunding_info_df = pd.read_excel('Resources/crowdfunding.xlsx')
 crowdfunding_info_df.head()
  
# Get a brief summary of the crowdfunding_info DataFrame.
 crowdfunding_info_df.info()
  
# Get the crowdfunding_info_df columns.
 crowdfunding_info_df.columns

# Get the crowdfunding_info_df unique category & sub-category.
 crowdfunding_category_df = crowdfunding_info_df["category & sub-category"].unique()
 crowdfunding_category_df
  
# Assign the category and subcategory values to category and subcategory columns.
 crowdfunding_info_df[["category","subcategory"]] = crowdfunding_info_df["category & sub-category"].str.split('/', n=1, expand=True)
 crowdfunding_info_df.head(10)
  
# Get the unique categories and subcategories in separate lists.
 categories = crowdfunding_info_df["category"].unique()
 subcategories = crowdfunding_info_df["subcategory"].unique()

 print(categories)
 print(subcategories)
  
# Get the number of distinct values in the categories and subcategories lists.
 print(len(categories))
 print(len(subcategories))
  
# Use a list comprehension to add "cat" to each category_id. 
 cat_ids = ["cat" + str(cat_id) for cat_id in category_ids]

# Use a list comprehension to add "subcat" to each subcategory_id.    
 subcat_ids = ["subcat" + str(scat_id) for scat_id in subcategory_ids ]
    
 print(cat_ids)
 print(subcat_ids)
  
# Create a category DataFrame with the category_id array as the category_id and categories list as the category name.
 category_df = pd.DataFrame({
     "category_id": cat_ids,
     "category" : categories
 })
  
# Create a category DataFrame with the subcategory_id array as the subcategory_id and subcategories list as the subcategory name. 
 subcategory_df = pd.DataFrame({
    "subcategory_id": subcat_ids,
    "subcategory" : subcategories
 })

# Export categories_df and subcategories_df as CSV files.
 category_df.to_csv("Resources/category.csv", index=False)
 subcategory_df.to_csv("Resources/subcategory.csv", index=False)
```

### Deliverable 2: Campaign DataFrame.

```python
# Create a copy of the crowdfunding_info_df DataFrame name campaign_df. 
 campaign_df = crowdfunding_info_df.copy()
 campaign_df.head()

# Rename the blurb, launched_at, and deadline columns.
  campaign_df.rename(columns={'blurb': "description",
                             'launched_at': "launched_date",
                              'deadline': "end_date"}, inplace=True)
  campaign_df.head(5)
 
# Convert the goal and pledged columns to a `float` data type.
  campaign_df["goal"]  = campaign_df["goal"].astype(float)
  campaign_df["pledged"]  = campaign_df["pledged"].astype(float)

  campaign_df.head(5)
 
# Check the datatypes
  campaign_df.info()

# Format the launched_date and end_date columns to datetime format
  from datetime import datetime as dt
  campaign_df["launched_date"] = pd.to_datetime(campaign_df["launched_date"], unit='s').dt.strftime('%Y-%m-%d') 
  campaign_df["end_date"] = pd.to_datetime(campaign_df["end_date"], unit='s').dt.strftime('%Y-%m-%d')

  campaign_df.head()

# Merge the campaign_df with the category_df on the "category" column and 
# the subcategory_df on the "subcategory" column.
  campaign_merged_df = campaign_df.merge(category_df, on = "category", how = "left").merge(subcategory_df, on = "subcategory", how = "left")
  campaign_merged_df.head()

# Drop unwanted columns
  campaign_cleaned = campaign_merged_df.drop(['staff_pick', 'spotlight', 'category & sub-category','category', 'subcategory'], axis=1)
  campaign_cleaned.head()

# Export the DataFrame as a CSV file. 
  campaign_cleaned.to_csv("Resources/campaign.csv", index=False)
``` 

### Deliverable 3: Contacts DataFrame.

```python
# Read the data into a Pandas DataFrame. Use the `header=2` parameter when reading in the data.
contact_info_df = pd.read_excel('Resources/contacts.xlsx', header=2)
contact_info_df.head()

# Importing regex, and creating a working copy of contact_info_df.
import re

contact_info_df_copy = contact_info_df.copy()
contact_info_df_copy.head()

# Removing header, and resetting the index.
working_df = contact_info_df_copy.rename(columns = contact_info_df_copy.iloc[0]).\
drop(contact_info_df_copy.index[0]).reset_index(drop = True)

working_df.head()


# Extracting contact_id, changing its datatype from string to integer, creating a list and appending contact_id into that list, and adding a new "contact_id" column into working dataframe.
contact_id = []
counter = 0

for i in working_df['contact_info']:
    row = working_df['contact_info'][counter]
    pattern = "\d+"
    cont_id = int(re.findall(pattern, row)[0])
    contact_id.append(cont_id)
    counter += 1

working_df['contact_id'] = pd.DataFrame(contact_id)
working_df.head(10)
# working_df.info() --> to check datatypes of dataframe values

# Extracting name from contact_info column, adding it into an empty list, and adding the list into working_df as a new column.
name = []
counter = 0

for i in working_df['contact_info']:
    row = working_df['contact_info'][counter]
    pattern = '\w+\s\w+'
    data = re.findall(pattern, row)
    name.append(data)
    counter += 1
    
working_df['name'] = pd.DataFrame(name)
working_df.head(10)

# Extracting email from contact_info column, adding it into an empty list, and adding the list into working_df as a new column.
email = []
counter = 0

for i in working_df['contact_info']:
    row = working_df['contact_info'][counter]
    pattern = '\w+\.\w+\@\w+\.\w+|\w+\@\w+\-\w+\.\w+'
    data = re.findall(pattern, row)
    email.append(data)
    counter += 1

working_df['email'] = pd.DataFrame(email)
working_df.head(10)

# Create a copy of the contact_info_df with the 'contact_id', 'name', and 'email' columns.
contact_info_df_cln = working_df.drop(columns = ['contact_info'])
contact_info_df_cln.head(10)

# Extracted first and last name from 'name' column, created new columbs for first_name and last_name, dropped 'name' column, and reordered the columns in the dataframe.
first_name = []
last_name = []
counter = 0

for i in contact_info_df_cln['name']:
    row = contact_info_df_cln['name'][counter]
    pattern = '\s'
    data1 = (re.split(pattern, row)[0])
    data2 = (re.split(pattern, row)[1])
    first_name.append(data1)
    last_name.append(data2)
    counter += 1

contact_info_df_cln['first_name'] = pd.DataFrame(first_name)
contact_info_df_cln['last_name'] = pd.DataFrame(last_name)
contact_info_df_cln.drop(columns = 'name')
contacts_df_clean = contact_info_df_cln[['contact_id', 'first_name', 'last_name', 'email']]
contacts_df_clean.head(10)

# Check the datatypes one more time before exporting as CSV file.
contacts_df_clean.info()

# Export the DataFrame as a CSV file. 
contacts_df_clean.to_csv("Resources/contacts.csv", encoding='utf8', index=False)

```

### Deliverable 4: Crowdfunding Database.

![image](https://github.com/fabiomarcullo/Crowdfunding_ETL/assets/123920059/d519c907-2a70-4c4c-9e0d-01a9395b5c72)


![image](https://github.com/fabiomarcullo/Crowdfunding_ETL/assets/123920059/847b1ba7-361c-42e3-80c6-004527c8f504)



