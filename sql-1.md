# A Day in the Life - Data Analyst
It's Catherine's first day working as a Data Analyst for ACME. She's excited to use some of the tools that she learned in Learn SQL from Scratch, such as:

Writing basic queries
Calculating aggregates
Combining data from multiple tables
Determining web traffic attribution
Creating usage funnels
Analyzing user churn

## Exploring Data with SQL
A database is a set of data stored in a computer. This data is usually structured into tables. Tables can grow large and have a multitude of columns and records.

Spreadsheets, like Microsoft Excel and Google Sheets, allow you to view and manipulate data directly: with selecting, filtering, sorting, etc. By applying a number of these operations you can obtain the subset of data you are seeking.

SQL (pronounced "S-Q-L" or "sequel") allows you to write queries which define the subset of data you are seeking. Unlike Excel and Sheets, your computer and SQL will handle how to get the data; you can focus on what data you would like. You can save these queries, refine them, share them, and run them on different databases.

Many databases use SQL (Structured Query Language). SQL is a great way to access data and a great entry point to programming because its syntax (the specific vocabulary that gives instructions to the computer) is very human-readable. Without knowing any SQL, you might still be able to guess what each command will do.

On her first day at ACME, Catherine wants to become familiar with the company's data, so she connects to the database and uses SQL to explore the database.

Instructions
1.
One of the tables in ACME's database is called browse. It contains information on each time someone visited the ACME's website. Paste the following code into the code editor (middle panel) and click "Run".

SELECT *
FROM browse
LIMIT 10;
This code will select all (*) columns from browse for the first 10 records.

```
SELECT *
FROM browse
LIMIT 10;
```

## Creating Usage Funnels
Visitors to ACME's website follow a simple workflow:

Browse items available for sale
Click an icon to begin the checkout process
Enter payment information to complete their purchase
Not all users who browse on the website will find something that they like enough to checkout, and not all users who begin the checkout process will finish entering their payment information to make a purchase.

This type of multi-step process where some users leave at each step is called a funnel.

Catherine wants to determine what percent of users make it through each step of the funnel so that she can recommend improvements to ACME's website.

Instructions
1.
Catherine is going to combine data from three different tables:

browse - gives the timestamps of users who visited different item description pages
checkout - gives the timestamps of users who visited the checkout page
purchase - gives the timestamps of when users complete their purchase
Using SQL, she finds that 24% of all users who browse move on to check out. 89% of those who reach checkout purchase.

```
SELECT ROUND(
   100.0 * COUNT(DISTINCT c.user_id) /
   COUNT(DISTINCT b.user_id)
 ) AS browse_to_checkout_percent,
 ROUND(
   100.0 * COUNT(DISTINCT p.user_id) /
   COUNT(DISTINCT c.user_id)
 ) AS checkout_to_purchase_percent
 FROM browse b
 LEFT JOIN checkout c
 	ON b.user_id = c.user_id
 LEFT JOIN purchase p
 	ON c.user_id = p.user_id;
```

## Analyzing User Churn
A churn rate is the percent of subscribers to a monthly service who have canceled. For example, in January, ACME has 1,000 customers. In February, 200 customers sign up, and 250 cancel. The churn rate for February would be: cancellations over subscribers equals 250 over 1000 plus 200 equals 20.8 percent

Catherine wants to analyze the churn rates for ACME for the past few months.

Instructions
1.
Click "Run", to see Catherine's analysis for the churn rate in March 2017.

What recommendations would you make to ACME based on Catherine's analysis?

```
SELECT COUNT(DISTINCT user_id) AS enrollments,
	COUNT(CASE
       	WHEN strftime("%m", cancel_date) = '03'
        THEN user_id
  END) AS march_cancellations,
 	ROUND(100.0 * COUNT(CASE
       	WHEN strftime("%m", cancel_date) = '03'
        THEN user_id
  END) / COUNT(DISTINCT user_id)) AS churn_rate
FROM pro_users
WHERE signup_date < '2017-04-01'
	AND (
    (cancel_date IS NULL) OR
    (cancel_date > '2017-03-01')
  );
```

## Determining Web Traffic Attribution
Catherine's boss asks her to analyze how users are finding ACME's websites using UTM Parameters. UTM Parameters are special tags that site owners add to their pages to track what website a user was on before they reach the website. For instance:

If a user found ACME's website through a Google search, the table page_visits might have utm_source equal to 'google'.
If a different user clicked a Facebook ad to get to ACME's website, then their row in page_visits might have utm_source equal to 'facebook'.
Instructions
1.
Catherine wants to know how many visits come from each utm_source.

```
SELECT utm_source,
 	COUNT(DISTINCT user_id) AS num_users
FROM page_visits
GROUP BY 1
ORDER BY 2 DESC;
```

## Todo
Writing basic queries
Calculating aggregates
Combining data from multiple tables
Creating usage funnels
Analyzing user churn
Determining web traffic attribution

##