![image](https://github.com/user-attachments/assets/93180310-8185-48dc-9604-d997d5394881)

# Yelp Data Streaming and Analytics Pipeline

This project implements a data streaming and analytics pipeline using various technologies, including TCP/IP Sockets, Apache Spark, Kafka, and Elasticsearch, for processing and visualizing Yelp data in real-time. Below is an overview of the architecture and its components.

![System Architecture](System_architecture_yelp.png)

## Components Description

### 1. Data Source: Yelp Dataset
- **Description**: We use the Yelp dataset as the initial data source for our pipeline. This dataset contains a rich collection of reviews, business details, and user ratings. It’s an ideal dataset to analyze customer behavior, business trends, and service quality based on real user feedback.
- **Data Details**: The dataset includes structured information like review text, star ratings, business categories, and location data. We utilize this data to simulate a real-world stream by reading it line by line and feeding it into our pipeline.
- **Purpose**: By using this data, we aim to process and analyze various aspects of customer reviews to gain meaningful insights that can be visualized in real-time.

### 2. TCP/IP Socket
- **Description**: A TCP/IP socket is used for streaming the Yelp data from the local storage to the Spark cluster. TCP (Transmission Control Protocol) and IP (Internet Protocol) are standard networking protocols used to establish reliable data transmission between client and server.
- **Data Streaming**: The Yelp dataset is broken down into smaller chunks and sent over the socket to simulate a live data stream. The socket acts as a continuous source of data, allowing us to mimic real-time data ingestion.
- **Functionality**: In our architecture, the TCP/IP socket serves as a bridge that feeds streaming data into the Spark processing engine, enabling seamless data flow for real-time analytics.

### 3. Apache Spark
- **Description**: Apache Spark is a distributed data processing engine used to handle large-scale data streams and batch processing. It’s particularly well-suited for real-time data processing due to its in-memory computation capabilities.
- **Architecture**:
  - The Spark setup consists of a **Master Node** and several **Worker Nodes**:
    - **Master Node**: Manages and coordinates the distributed tasks across the cluster.
    - **Worker Nodes**: Execute the actual data processing, including filtering, aggregations, and transformations on incoming streams.
- **Use in Pipeline**: Spark’s built-in streaming libraries are used to perform transformations and analyze incoming Yelp reviews. For example, we can aggregate reviews by business category, calculate average ratings, and identify trends in customer feedback.

### 4. Confluent Kafka
- **Description**: Confluent Kafka is a cloud-based implementation of Apache Kafka, providing a reliable and scalable messaging system. Kafka handles data streams between the different components of our architecture, ensuring data is transmitted efficiently and reliably.
- **Functionality**: 
  - Kafka is used to **ingest, buffer, and stream** data between Spark and Elasticsearch. It enables the flow of processed data from Spark to downstream consumers.
  - Each topic in Kafka represents a different category of data, allowing for easy separation and organization of various data streams.
- **Cluster on the Cloud**: Using Confluent Kafka’s cloud-hosted services, we gain the advantage of managed Kafka infrastructure, which simplifies scaling and managing our Kafka brokers.

### 5. Control Center and Schema Registry
- **Control Center**:
  - A web-based management and monitoring tool provided by Confluent, used for tracking the health of the Kafka clusters, topics, and messages.
  - It allows us to visualize message throughput, consumer lag, and topic configurations, providing real-time insights into the state of the Kafka cluster.
- **Schema Registry**:
  - Manages schemas for the data flowing through Kafka topics, ensuring data compatibility and preventing schema evolution issues.
  - Every piece of data in Kafka is stored along with its schema, allowing for strict enforcement of data contracts, which is crucial for maintaining data quality and integrity as our pipeline evolves.

### 6. Kafka Connect
- **Description**: Kafka Connect is a tool for **scalable and reliable streaming data between Kafka and other data systems**. It provides a framework to connect Kafka topics to various external systems, such as databases and search engines.
- **Use in Pipeline**: In our project, Kafka Connect is configured to transfer the data from Kafka to Elasticsearch.
- **Connector Type**:
  - We utilize a **sink connector** that reads processed data from Kafka topics and streams it to Elasticsearch in near real-time, making it immediately available for indexing and querying.

### 7. Elasticsearch
- **Description**: Elasticsearch is a distributed search and analytics engine designed for handling large volumes of data with near real-time search capabilities. It indexes incoming data to provide fast and efficient querying capabilities.
- **Functionality**: 
  - **Indexing**: Each piece of data ingested from Kafka is stored as a document in Elasticsearch indices. These indices are structured and optimized for text searches, making Elasticsearch ideal for analyzing Yelp reviews.
  - **Querying**: We can perform complex queries on the data, such as full-text search on review text, filtering by business category, or aggregating star ratings.
- **Use Case**: Elasticsearch serves as our primary data store, where processed Yelp reviews are indexed and made available for further analysis.

### 8. Visualization: Kibana, Power BI, and Tableau
- **Kibana**: 
  - Kibana is a visualization tool that integrates seamlessly with Elasticsearch. It provides interactive dashboards, search interfaces, and real-time visualization of the Yelp review data.
  - Use Case: We use Kibana to build visual dashboards that show key metrics such as review trends, sentiment analysis, and top-rated businesses.
  
- **Power BI**:
  - Power BI connects to Elasticsearch to build more business-centric visualizations and reports.
  - Use Case: It’s used for creating client-facing reports that showcase data trends, providing high-level business insights.
  
- **Tableau**:
  - Tableau provides advanced visual analytics and interactive dashboards for deeper data exploration.
  - Use Case: In our architecture, Tableau is used to create customized visualizations and complex data aggregations, allowing for detailed drill-downs into specific business segments or customer feedback trends.
