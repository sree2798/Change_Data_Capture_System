# Change_Data_Capture_System
## Overview
This project demonstrates a real-time Change Data Capture (CDC) system using Debezium, Kafka, and PostgreSQL, hosted in a Docker environment. It captures database changes and streams them to Kafka topics, enabling real-time processing and analytics.

## Features
- Captures inserts, updates, and deletes from a PostgreSQL database.
- Streams database changes to Kafka brokers using Debezium.
- Visualizes data changes in real-time using Debezium UI and Kafka Control Center.
- Tracks metadata (e.g., user modifications and timestamps) for enhanced data auditability.
- Supports decimal value serialization and ensures data integrity.
## Tech Stack
- Languages: Python
- Database: PostgreSQL
- Streaming: Apache Kafka
- CDC Tool: Debezium
- Orchestration: Docker, Docker Compose
- IDE: PyCharm
## Setup 
### Prerequisites
- Install Docker and Docker Compose.
### Running the project
- start the docker containers
```bash
docker-compose up -d
```
- verify services
  - kafka control center: http://localhost:9021
  - Debezium UI: http://localhost:8080
### Database Setup
- Access the PostgreSQL container:
```bash
  docker exec -it postgres /bin/bash
```
- connect to the database:
```bash
  psql -U postgres -d financial_db
```
- Create the transactions table using the included Python script
```python
python main.py
```
### Usage
- #### step 1: Simulate Transactions
```python
python main.py
```
- #### step 2:  Create a Debezium Connector
Use the provided JSON configuration to create a Debezium connector for PostgreSQL.
  ```json
  {
  "name": "postgres-connector",
  "config": {
    "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
    "plugin.name": "pgoutput",
    "topic.prefix": "cdc",
    "database.hostname": "postgres",
    "database.port": "5432",
    "database.user": "postgres",
    "database.password": "password",
    "database.dbname": "financial_db",
    "decimal.handling.mode": "string"
  }
  }
```
#### Step 3: Observe CDC in Action
- Perform database operations (insert, update, delete)
- View changes streamed to Kafka topics in the Kafka Control Center or consume them using a Kafka consumer.
### Key Configurations
This project uses Docker Compose to orchestrate services:
- PostgreSQL
- Zookeeper
- Kafka Broker
- Debezium
- Debezium UI
- Kafka Control Center
Refer to the docker-compose.yml file for detailed configurations.

### PostgreSQL REPLICA IDENTITY
Configured for full row logging:
```sql
ALTER TABLE transactions REPLICA IDENTITY FULL;
```

  
