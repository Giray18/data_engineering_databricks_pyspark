# data_engineering
This Repo contains activities related to ELT, data warehouse creation and advanced analytics by Spark framework (PySpark on Databricks)

# Task:
There are table definitions which has structure of source tables without any data sample only DDL sql script is available and can be found at files with name 'test_tables 1.sql'.
Tables belongs to gaming domain on app user and game activities

-IN_APP_PURCHASE_LOG_SERVER (Covers user item purchasing data)
-MULTIPLAYER_BATTLE_STARTED (Covers multiplayer battle related data)
-LOGIN (Covers user login data to app)
-NEW_USER (Covers new users user data)
-SESSION_STARTED (Covers session data when user created session)
-SHIP_TRANSACTION_LOG (Covers in game ship buy-purchase-trade data from users items)

# Current Stage:
Based on DDL sql script shared with faker package some fake source tables created with notebook called "Bronze_Layer_Notebook".
For to use 

# Target Stage:
Intention is to create 3 tier architecture on delta lakehouse architecture.
-Bronze layer will hold ingested tables as raw data format
-Silver layer will hold cleaned and relatively normalized tables as close as It can get to 3NF
-Gold layer will hold analytics datawarehouse formatted tables and aggregated views as business requirements mentioned below.
  - All metrics are requested to be calculated by time periods required as Daily,Monthly and Weekly (Prefilters final fact table before metrics calculated)

## General Metrics
Active Users: Unique User count exists on f_multi_ships table based on field named "Session_User_Id"
New Users: Unique User count exists on d_new_user dimension table based on field named "User_User_ID"
Revenue Sum: Total revenue sum from IN APP user purchasements calculated based on f_multi_ships table`s field "IN_APP_USD_COST" sum(IN_APP_USD_COST)
Spender Users: Count of User_Is_Spender field from d_user_id dim table
ARPU: Revenue per active user, calculated by division of Total Revenue to Active User Number
ARRPU: Revenue per spender users, calculated by division of Total Revenue to Spender User Number
1 Day Retention Rate: Division of game played new user number to all new user number for last 1 day
3 Day Retention Rate: Division of game played new user number to all new user number for last 3 day
7 Day Retention Rate: Division of game played new user number to all new user number for last 7 day
7 Day Conversion Rate: Division of item purchased new user number to all new user number for last 7 day

## In game ship related metrics
Ships owned by everyuser everyday: Ships count that get into transactions this view will show that count as grouped by user_id, daily timestamp and ship name
Daily ships popularity: Ships rank by purchase count daily / Ships rank by sold count daily

## User transactions overview
Amount of multiplayer battles before first purchase date of users: User battle participation count before their first purchase date
Amount of logins before first purchase date of users: User login count before their first purchase date
Amount of days before first purchase date of users: User day count before their first purchase date (Between their registration date and first purchase date)
Daily revenue per user: IN_APP item cost sum per User -- IN_APP_USD_COST field sum grouped by USER_ID for last 1 day
Weekly revenue per user: IN_APP item cost sum per User -- IN_APP_USD_COST field sum grouped by USER_ID for last 7 day
Monthly revenue per user: IN_APP item cost sum per User -- IN_APP_USD_COST field sum grouped by USER_ID for last 30 day

## Battle analysis
New users participation in battles since 1/3/7/14 days since registration: User table with battle participation count as daily
Active users battle participations of all times: User table with battle participation count as daily

