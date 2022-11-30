# Amazon_Vine_Analysis

## OVERVIEW 

In this project, we are going to take a look at Amazon Reviews for Video DVDs. 
First, we will perform an Extract-Transform-Load (ETL) to extract the dataset provided to us, transform the data and connect it to an Amazon Web Services Relational Database Sevice (AWS RDS) and loading the data into pgAdmin, using PySparks. We will also determine if there is a buas towards favorable reviews from *Vine Members** in the dataset used.  

##### *The Amazon Vine Program is a service that allows manufactureres and publishers to get Amazon reviews on their products. The company pays a small fee to Amazon and provides the products to Amazon Vine members for their honest reviews.  
We are interested to see how the reviews from Vine members affect the favorability of the product.  

## DELIVERABLE 1

In order to go through with this project, there are a few things you need to set up:

  * You will need an AWS account, in which you will proceed to create a Database. While your database settings will make it public, it is still protected with a password of your choosing.
  
  * You will then open pgAdmin and create a new Server Connection using your Database Endpoint as the Host Name/Address. A new Database is then created here as well. The password selected earlier will be used in pgAdmin to ensure the connection is secure. 
  
Once these steps have been fulfilled, we can start creating the tables we will need for this analysis.
  
  There are 4 tables we need to create:
  
   1. Review ID table  
    
   2. Customers table  
    
   3. Products table
    
   4. Vine table
   
Now that the tables have been created, we are going to start our ELT process in a new Google Colab Notebook. We will be using pySpark for this project.

Please feel free to check out the code: [Amazon_Reviews_ELT](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Amazon_Reviews_ELT.ipynb)

1. We must make all the necessary installments/imports.

2. We will bring in the Amazon Database of our choice. (This project will look into Video DVD Reviews). This will be transformed into a dataframe.

![video_review_df](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/video_review_ELT.PNG)

3. Next, we will create tables that are congruent with their pgAdmin table counterparts. It is important to select only the columns that are in the pgAdmin tables.

Customers DataFrame

![customers_df](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/customers_df.PNG)

Here, we have to make sure to change the Column header name to "customer_count".

Products DataFrame

![products_df](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/products_df.PNG)

Notice we drop any duplicate product_id in this step. Product ID is the Primary key in the pgAdmin Products table therefore any duplicates will throw an error.

Review ID DataFrame

![review_id_df](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/review_id_df.PNG)

In this case, we must make sure the review_date column in is 'yyyy-MM-dd' format.

Vine DataFrame

![vine_df](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/vine_df.PNG)

4. Now we will connect to the AWS RDS and load each dataframe into the respective pgAdmin table.

![connection+to AWS RDS](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/connection.PNG)

- For jbdc_url, use the Endpoint Connection for your database. You can find this on AWS or your pgAdmin Server properties (under Connection>> Host Name/Address)   

- The first cell is created to temporarily hold your database password, without divulging it, for security purposes. Type in your database password to form the connection.  

- You will now proceed to load each dataframe to the tables.

5. Once completed, open pgAdmin to ensure the database has been loaded.

Customers Table

![customers_table](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/customer_table.PNG)

Products Table

![products_table](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/products_table-sql.PNG)

Review ID Table

![review_id_table](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/review_id.PNG)

Vine Table

![vine_table](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/vine_table-sql.PNG)

This concludes the first part of our project.

### DELIVERABLE 2
 
We are now going to get into the Analysis portion of our project. We want to see how the Vine Reviews affect the product reviews as a whole. 

The code for this portion of the project can be found here: [Vine_Review_Analysis](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Vine_Review_Analysis.ipynb)

1. We will once again start by making the necessary imports and installments.  

2. Once again, create a dataframe of the Video DVDs database.

![video_review_df](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/video_review_vine.PNG)

3. We will filter the video_reviews_df to retrieve only rows where  

       total_votes count >= 20
       
   These are the most helpful comments and would thereby, most likley make the most impact.

![total_votes](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/total_votes_df.PNG)

4. Next, filter total_votes to retrieve the rows where  

       number of helpful votes/total_votes >= 0.5
    
 ![helpful_votes](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/helpful_votes_df.PNG)
   
5. Now filter the helpful_votes_df to retrieve all the rows that are enrolled in the Vine program  

        vine == Y
    
  ![vine_df](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/vine_df_vine.PNG)
  
 6. Repeat the step for all the rows that are not enrolled in the Vine program  
 
        vine == N
    
  ![not_vine_df](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/not_vine_df.PNG)
    
 7. Determine the following:
 
    * Total Number of Paid and Unpaid Reviews   
    * Total number of 5 Star Paid and Unpaid Reviews  
    * Percentage of 5-star Paid and Unpaid Reviews
    
  ![statistics](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/stats.PNG)
  
  
 ## Analysis
 
 Let us take a look at these numbers to further analyze them.
 
 ![prints](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/prints.PNG)
 
 From this, we can see that there are only 49 helpful, paid reviews, whereas a whopping 151400 reviews are unpaid!  
 That being said, only 18% (9 reviews) of the paid reviews are 5 star reviews.    
 On the other hand, 5% (78061 reviews) of the unpaid reviews are 5 star reviews.   
 
 From this, we can see: 
 
  * There are a very few number of useful Vine member reviews in this database. Of them, a very small number of the reviews are 5 stars.  
  * There is a larger number of helpful, 5 star reviews from unpair reviewers.  
 Therefore, in this dataset, the Vine member Reviews do not cause a bias in overall rating of Video DVDs.
 
 ### Additional Analysis
 
 Let us check how many Paid and Unpaid reviewers there are. Earlier, we only considered the helpful paid and unpaid reviews. A look at this can give us an idea of how many paid customers have been reached out to.  
 Let us take a look at these numbers:
 
 ![additional_data](https://github.com/SoumyaAbraham/Amazon_Vine_Analysis/blob/main/Images/addition.PNG)
 
 These numbers show us that less than 1% of the total dataset consists of Paid/Vine members. This strongly supports the conclusion that a bias cannot be seen in this dataset.
 
 
