# SQL-DRIVEN-MENTOR-PERFORMANCE-TRACKER

🧩 Project Synopsis
This project aims to guide aspiring data analysts through real-world SQL data analysis by utilizing a structured dataset from an educational platform. The primary objective is to perform insightful evaluations of user behavior and submission performance through a sequence of SQL-based tasks.

🎯 Key Goals
Gain practical exposure to SQL for analytical operations such as summarization, filtering, and user-based ranking.

Learn to calculate metrics and derive insights from activity-based datasets.

Strengthen hands-on expertise with SQL functions like COUNT(), SUM(), AVG(), EXTRACT(), and DENSE_RANK().

Perform user performance tracking and error analysis using advanced SQL techniques.

👥 Audience Level: Entry-Level
Tailored for newcomers with foundational SQL understanding, this project is ideal for developing competence in analyzing structured data. It features manageable datasets and practical query problems reflecting real analytical scenarios.

## SQL Mentor User Performance Dataset

The dataset consists of information about user submissions for an online learning platform. Each submission includes:
- *User ID*
- *Question ID*
- *Points Earned*
- *Submission Timestamp*
- *Username*

This data allows you to analyze user performance in terms of correct and incorrect submissions, total points earned, and daily/weekly activity.

## SQL Problems and Questions

Here are the SQL problems that you will solve as part of this project:

### Q1. List All Distinct Users and Their Stats
- *Description*: Return the user name, total submissions, and total points earned by each user.
- *Expected Output*: A list of users with their submission count and total points.

### Q2. Calculate the Daily Average Points for Each User
- *Description*: For each day, calculate the average points earned by each user.
- *Expected Output*: A report showing the average points per user for each day.

### Q3. Find the Top 3 Users with the Most Correct Submissions for Each Day
- *Description*: Identify the top 3 users with the most correct submissions for each day.
- *Expected Output*: A list of users and their correct submissions, ranked daily.

### Q4. Find the Top 5 Users with the Highest Number of Incorrect Submissions
- *Description*: Identify the top 5 users with the highest number of incorrect submissions.
- *Expected Output*: A list of users with the count of incorrect submissions.

### Q5. Find the Top 10 Performers for Each Week
- *Description*: Identify the top 10 users with the highest total points earned each week.
- *Expected Output*: A report showing the top 10 users ranked by total points per week.

## Key SQL Concepts Covered

- *Aggregation*: Using COUNT, SUM, AVG to aggregate data.
- *Date Functions*: Using EXTRACT() and TO_CHAR() for manipulating dates.
- *Conditional Aggregation*: Using CASE WHEN to handle positive and negative submissions.
- *Ranking*: Using DENSE_RANK() to rank users based on their performance.
- *Group By*: Aggregating results by groups (e.g., by user, by day, by week).

## SQL Queries Solutions

Below are the solutions for each question in this project:

### Q1: List All Distinct Users and Their Stats
sql
SELECT 
    username,
    COUNT(id) AS total_submissions,
    SUM(points) AS points_earned
FROM user_submissions
GROUP BY username
ORDER BY total_submissions DESC;


### Q2: Calculate the Daily Average Points for Each User
sql
SELECT 
    TO_CHAR(submitted_at, 'DD-MM') AS day,
    username,
    AVG(points) AS daily_avg_points
FROM user_submissions
GROUP BY 1, 2
ORDER BY username;


### Q3: Find the Top 3 Users with the Most Correct Submissions for Each Day
sql
WITH daily_submissions AS (
    SELECT 
        TO_CHAR(submitted_at, 'DD-MM') AS daily,
        username,
        SUM(CASE WHEN points > 0 THEN 1 ELSE 0 END) AS correct_submissions
    FROM user_submissions
    GROUP BY 1, 2
),
users_rank AS (
    SELECT 
        daily,
        username,
        correct_submissions,
        DENSE_RANK() OVER(PARTITION BY daily ORDER BY correct_submissions DESC) AS rank
    FROM daily_submissions
)
SELECT 
    daily,
    username,
    correct_submissions
FROM users_rank
WHERE rank <= 3;


### Q4: Find the Top 5 Users with the Highest Number of Incorrect Submissions
sql
SELECT 
    username,
    SUM(CASE WHEN points < 0 THEN 1 ELSE 0 END) AS incorrect_submissions,
    SUM(CASE WHEN points > 0 THEN 1 ELSE 0 END) AS correct_submissions,
    SUM(CASE WHEN points < 0 THEN points ELSE 0 END) AS incorrect_submissions_points,
    SUM(CASE WHEN points > 0 THEN points ELSE 0 END) AS correct_submissions_points_earned,
    SUM(points) AS points_earned
FROM user_submissions
GROUP BY 1
ORDER BY incorrect_submissions DESC;


### Q5: Find the Top 10 Performers for Each Week
sql
SELECT *  
FROM (
    SELECT 
        EXTRACT(WEEK FROM submitted_at) AS week_no,
        username,
        SUM(points) AS total_points_earned,
        DENSE_RANK() OVER(PARTITION BY EXTRACT(WEEK FROM submitted_at) ORDER BY SUM(points) DESC) AS rank
    FROM user_submissions
    GROUP BY 1, 2
    ORDER BY week_no, total_points_earned DESC
)
WHERE rank <= 10;


## Conclusion

This project provides an excellent opportunity for beginners to apply their SQL knowledge to solve practical data problems. By working through these SQL queries, you'll gain hands-on experience with data aggregation, ranking, date manipulation, and conditional logic..... This is a read me file of a project I have to give the exact file for my project... So rephrase all these words technically... Create a redmi file accordingly don't use repeated words use the same logic but don't use the word wordings similar to this now give me the full read me file 
