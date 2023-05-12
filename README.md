# Project 2

## Background

For the ETL mini project, you will work with a partner to practice building an ETL pipeline using Python, Pandas, and either Python dictionary methods or regular expressions to extract and transform the data. After you transform the data, you'll create four CSV files and use the CSV file data to create an ERD and a table schema. Finally, you’ll upload the CSV file data into a Postgres database.

Since this is a one-week project, make sure that you have done at least half of your project before the third day of class to stay on track.

Although you and your partner will divide the work, it’s essential to collaborate and communicate while working on different parts of the project. Be sure to check in with your partner regularly and offer support.

## Data Source

1. [Contacts Data Source](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/Resources/contacts.xlsx)

2. [Crowdfunding Data Source](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/Resources/crowdfunding.xlsx)

## Codes

- [ETL_Mini_Project_FLima_JArambulo](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/ETL_Mini_Project_FLima_JArambulo.ipynb)

## Output

- [Category Data Frame](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/Resources/category.csv)
- [Subcategory Data Frame](https://github.com/fabiomarcullo/Crowdfunding_ETL/tree/main/Resources/subcategory.csv)

## Project Deliverables

  ### Deliverable 1: Category DataFrame.

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

### Deliverable 2: Subcategory DataFrame.

a. 

b. 

### Deliverable 3: Campaign DataFrame.

a. 

b. 

### Deliverable 4: Contacts DataFrame.

a. 

b. 

### Deliverable 5: Crowdfunding Database.

a. 

b. 


