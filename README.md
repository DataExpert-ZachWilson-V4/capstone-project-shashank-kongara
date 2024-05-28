[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/1lXY_Wlg)

# Project Proposal: End-to-End Data Engineering with Football Data

## 1. Problem Identification

**Problem Statement:**
Analyze and predict player performance and club success based on historical football data to aid in strategic decisions for team management, player transfers, and match preparations.

**Objective:**
The primary objective is to create a comprehensive data pipeline that ingests, processes, and stores football data from multiple sources. This pipeline will support the development of analytics tools and dashboards to gain insights into player performance, team dynamics, and match outcomes.

## 2. Project Scope and Datasets

**Scope:**
The project will focus on building a data pipeline that covers the following steps:
- Data Ingestion
- Data Cleaning and Transformation
- Data Storage
- Data Analysis and Visualization

**Datasets:**
- **CSV Files:**
  - appearances.csv
  - club_games.csv
  - game_events.csv
  - games.csv
  - player_valuations.csv
  - players.csv

- **JSON Files:**
  - [clubs.json] (converted from clubs.csv)
  - [competitions.json] (converted from competitions.csv)

- **External Data Sources:**
  - **APIs:** Real-time match data from an external sports API (e.g., Football Data API)

The combined data from these sources will exceed 1 million rows.

## 3. Use Cases for Data Preparation

The prepared data will be used for the following purposes:
- **Analytics Table in a Data Warehouse:**
  - To store aggregated data for historical analysis and reporting
- **Relational Database:**
  - To enable complex queries and relationship-based analysis between different entities (players, clubs, matches)
- **Dashboard:**
  - To visualize key performance indicators (KPIs) for players and teams, using tools like Tableau or Power BI

## 4. Tech Stack and Tool Choices

**Tech Stack:**
- **Data Ingestion:**
  - **Apache Kafka:** For real-time data streaming from APIs
  - **Apache NiFi:** For data ingestion and integration
- **Data Storage:**
  - **Amazon S3:** For storing raw and processed data
  - **Amazon Redshift:** For creating an analytics data warehouse
  - **PostgreSQL:** For the relational database
- **Data Processing:**
  - **Apache Spark:** For distributed data processing and transformation
  - **AWS Glue:** For ETL (Extract, Transform, Load) operations
- **Data Visualization:**
  - **Tableau:** For creating interactive dashboards
  - **Power BI:** For business intelligence and reporting
- **Data Orchestration:**
  - **Apache Airflow:** For workflow automation and scheduling

**Justification for Tech Stack:**
- **Apache Kafka and NiFi:** Efficient for handling large volumes of streaming data and integrating various data sources.
- **Amazon S3 and Redshift:** Highly scalable and suitable for storing large datasets and performing complex queries.
- **PostgreSQL:** Reliable and widely used relational database with robust support for complex queries.
- **Apache Spark:** Capable of processing large datasets quickly and efficiently.
- **Tableau and Power BI:** Leading tools for data visualization and business intelligence, offering rich features for creating insightful dashboards.
- **Apache Airflow:** Ideal for managing and automating data workflows, ensuring seamless data processing and integration.

## 5. Data Modeling

### Incremental Tables

**Fact Tables:**
1. **Fact_Appearances:**
   - Columns: `appearance_id`, `game_id`, `player_id`, `player_club_id`, `date`, `competition_id`, `yellow_cards`, `red_cards`, `goals`, `assists`, `minutes_played`

2. **Fact_Game_Events:**
   - Columns: `game_event_id`, `game_id`, `minute`, `type`, `club_id`, `player_id`, `description`, `player_in_id`, `player_assist_id`

**Dimension Tables:**
1. **Dim_Players:**
   - Columns: `player_id`, `first_name`, `last_name`, `name`, `current_club_id`, `country_of_birth`, `city_of_birth`, `country_of_citizenship`, `date_of_birth`, `sub_position`, `position`, `foot`, `height_in_cm`, `contract_expiration_date`, `agent_name`, `image_url`, `market_value_in_eur`, `highest_market_value_in_eur`

2. **Dim_Clubs:**
   - Columns: `club_id`, `club_code`, `name`, `domestic_competition_id`, `total_market_value`, `squad_size`, `average_age`, `foreigners_number`, `foreigners_percentage`, `national_team_players`, `stadium_name`, `stadium_seats`, `net_transfer_record`, `coach_name`, `last_season`, `url`

3. **Dim_Competitions:**
   - Columns: `competition_id`, `competition_code`, `name`, `sub_type`, `type`, `country_id`, `country_name`, `domestic_league_code`, `confederation`, `url`, `is_major_national_league`

4. **Dim_Games:**
   - Columns: `game_id`, `competition_id`, `season`, `round`, `date`, `home_club_id`, `away_club_id`, `home_club_goals`, `away_club_goals`, `home_club_position`, `away_club_position`, `home_club_manager_name`, `away_club_manager_name`, `stadium`, `attendance`, `referee`, `home_club_formation`, `away_club_formation`, `home_club_name`, `away_club_name`, `aggregate`, `competition_type`

### Cumulative Table

**Cumulative_Player_Stats:**
- Columns: `player_id`, `player_name`, `current_club_name`, `country_of_birth`, `country_of_citizenship`, `date_of_birth`, `position`, `seasons (ARRAY)`, `is_active`, `years_since_last_active`, `current_season`
- **Seasons Array Structure:** Each season includes `season`, `goals`, `assists`, `yellow_cards`, `red_cards`, `minutes_played`

### Slowly Changing Dimension (SCD) Type 2 Table

**SCD_Dim_Players:**
- Columns: `surrogate_key`, `player_id`, `player_name`, `current_club_name`, `country_of_birth`, `country_of_citizenship`, `date_of_birth`, `position`, `market_value_in_eur`, `start_date`, `end_date`, `is_current`, `current_season`
- **Description:** Tracks historical changes in player attributes by creating new records with new surrogate keys whenever changes occur.

## 6. Conclusion

This project aims to leverage diverse datasets and advanced data engineering tools to build a robust data pipeline. The end goal is to provide actionable insights into player performance and team success, aiding strategic decision-making in football management.
