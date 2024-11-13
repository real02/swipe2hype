# Swipe2hype project roadmap

### 1. **Problem Statement and Scope**

**Problem:**  
We want to provide real-time insights into swipe activity on a dating app, allowing for location-based insights, targeted marketing, and optimization of matchmaking algorithms. By tracking swipe behavior, location data, and user activity in real-time, we can generate valuable business insights, such as popular user profiles, regional activity surges, and optimal engagement times.

**Scope:**  
Set up a real-time data pipeline that ingests swipe events from a Kafka stream, processes it with Spark Streaming, validates and cleans the data with Great Expectations, and stores the processed data in a format ready for analytics. We'll create an analytics dashboard for real-time visualizations to track swipe activity and provide location-based insights.

### 2. **Data Generation and Sources**

Since we’re creating our own dataset, we’ll simulate swipe activity with a Python script to continually send events to Kafka. Here’s the plan for generating and storing the data:

1. **Kafka (Streaming Source):**
   - Simulate swipe events with fields like `user_id`, `swipe_direction`, `profile_id`, `timestamp`, `location`, and possibly more.
   - Format: JSON (streamed into Kafka).
2. **Static Data Sources (for Enrichment):**
   - User profiles and location metadata could be stored as CSV or Parquet files.
   - Profiles may include fields like `profile_id`, `age`, `interests`, and `city`.
   - Locations could map GPS coordinates to neighborhoods or regions.

### 3. **Tech Stack and Tools Justification**

- **Python**: To generate data and write the initial data ingestion scripts.
- **Kafka**: For real-time streaming of swipe events; scalable and resilient for handling continuous data.
- **Spark Streaming**: Processes real-time swipe data, enriches it with static data, and aggregates key metrics.
- **Great Expectations**: Ensures data quality, with checks on format, completeness, and referential integrity.
- **AWS (Managed Kafka and Deployment)**: For managed Kafka (Amazon MSK) and potential use of S3, Redshift, or RDS to store analytics-ready data.
- **Dashboarding Tool (e.g., Grafana or Kibana)**: Visualizes real-time analytics, allowing users to interact with trends and location-based insights.

### 4. **Data Pipeline Architecture**

Here's the flow of the data pipeline:

1. **Data Generation and Kafka Ingestion**:
   - Use a Python script to create swipe event data and send it to Kafka.
2. **Spark Streaming Processing**:
   - Spark consumes data from Kafka in micro-batches, processes the events, and enriches them using profile and location data.
   - Aggregate data by swipe type, region, and time window (e.g., hourly).
3. **Data Validation (Great Expectations)**:
   - Add checks at each step to ensure data quality, including format validation, field checks, referential integrity, and sanity checks for high/low volume events.
4. **Storage for Analytics**:
   - Store cleaned, processed data in a data warehouse (e.g., AWS Redshift) or a real-time store (e.g., Elasticsearch or Clickhouse for high-speed analytics).
5. **Visualization**:
   - Use Grafana or Kibana for a real-time dashboard. Display metrics like swipe counts by location, active user counts, and alert notifications for activity spikes.

### 5. **Data Quality Checks with Great Expectations**

Set up these essential data quality checks:

- **Sanity Checks**: Ensure swipe events are realistic in count and variety.
- **Referential Integrity**: Verify that `profile_id` in swipe events matches an ID in the profile data.
- **Field Format**: Validate fields (e.g., `swipe_direction` only accepts 'left' or 'right').
- **Missing Data**: Ensure critical fields like `user_id` and `location` are populated.

### 6. **ETL Pipeline and Data Transformation**

Here’s a basic outline of the ETL pipeline code for Spark:

- **Data Ingestion**: Spark Streaming reads from Kafka topic.
- **Transformation**: Parse JSON, map `location` data, join with static profile data, and create aggregates.
- **Validation**: Run Great Expectations checks on transformed data.
- **Storage**: Load into Elasticsearch (for real-time) and/or AWS Redshift for deeper analysis.

### 7. **Diagramming the Target Data Model**

Create a diagram with tools like Lucidchart or dbdiagram.io:

- **Fact Table**: Swipes with columns `user_id`, `profile_id`, `swipe_direction`, `location`, and `timestamp`.
- **Dimension Tables**: Profiles and locations, with attributes for enrichment (e.g., age, interests, neighborhood).

### 8. **Data Dictionary**

Document fields for clarity, including field names, types, constraints, and descriptions. This will serve as a reference to ensure consistency and accuracy in data processing.

### 9. **Code for Real-Time Pipeline Execution and Monitoring**

For code execution:

- Write Spark code to run as a recurring job using Spark’s structured streaming API.
- Set up data quality checks with Great Expectations in Python, integrated with Spark.
- For monitoring, use Grafana to create alerts based on real-time data thresholds (e.g., surges in activity).
