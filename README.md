# Log Processing Using Lambda Architecture

![Architecture](image/Log%20Analyst%20Architecture.png)


## 🎯 Project Overview

This project demonstrates a **real-time log analysis system** using Lambda Architecture to process NASA access logs. The system provides real-time insights through web server log analysis for network troubleshooting, security monitoring, and performance optimization.

### Why Log Analysis?

Web server log analysis offers critical insights for:
- **Network troubleshooting** efforts
- **Development and quality assurance**
- **Security issues** identification and understanding
- **Customer service** improvements
- **Compliance** with government and corporate policies

## 🏗️ Architecture

### Data Flow Architecture
The project implements **Lambda Architecture** with:
- **Speed Layer**: Real-time processing with Kafka + Spark Streaming → Cassandra
- **Batch Layer**: Batch processing → HDFS
- **Serving Layer**: Unified view through Plotly + Dash visualization

### Tech Stack
- **Cloud**: AWS EC2
- **Containerization**: Docker & Docker Compose
- **Data Ingestion**: Apache NiFi
- **Message Streaming**: Apache Kafka
- **Stream Processing**: Apache Spark Structured Streaming
- **Databases**: 
  - Cassandra (Speed Layer)
  - HDFS (Batch Layer)
- **Visualization**: Plotly + Dash
- **Development**: Jupyter Lab, Python

## 🚀 Quick Start

### Prerequisites
- AWS EC2 instance (t2.xlarge recommended)
- 32GB storage minimum
- SSH access configured

### 1. Environment Setup

#### EC2 Instance Configuration
```bash
# Allow ports 4000-38888 in security group
# Connect via SSH with port forwarding
ssh -i "your-key.pem" ec2-user@your-ec2-dns \
    -L 2081:localhost:2041 \
    -L 4888:localhost:4888 \
    -L 2080:localhost:2080 \
    -L 8050:localhost:8050 \
    -L 4141:localhost:4141
```

#### Docker Installation
```bash
sudo yum update -y
sudo yum install docker
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo gpasswd -a $USER docker
newgrp docker

# Start Docker
sudo systemctl start docker
```

### 2. Start Services
```bash
# Start all services using docker-compose
docker-compose up -d

# Verify services are running
docker ps
```

### 3. Access Services
- **NiFi**: http://localhost:2080/nifi/
- **Jupyter Lab**: http://localhost:4888/lab
- **HDFS**: http://localhost:50070/
- **Dash Application**: http://localhost:8050/ (available after running log_visualizer.ipynb)

## 📊 Data Pipeline

### 1. Data Extraction
#### Download Dataset
- **Source**: [NASA Access Logs](https://ita.ee.lbl.gov/html/contrib/NASA-HTTP.html)
- **Kaggle**: [NASA Access Log Dataset](https://www.kaggle.com/souhagaa/nasa-access-log-dataset-1995/download)

![Nifi Workflow](image/Nifi%20Workflow.png)

### 2. Kafka Topic Creation
```bash
# Enter Kafka container
docker exec -i -t kafka bash

# Create topic
kafka-topics.sh --create --topic nasa_logs_demo \
    --partitions 1 --replication-factor 1 \
    --if-not-exists --zookeeper zookeeper:2181

# List topics
kafka-topics.sh --list --bootstrap-server localhost:29092

# Test consumer
kafka-console-consumer.sh --bootstrap-server localhost:29092 \
    --topic nasa_logs_demo --from-beginning --max-messages 30
```

### 3. Cassandra Setup
```bash
# Enter Cassandra container
docker exec -i -t cassandra bash
cqlsh -u cassandra -p cassandra

# Create keyspace and table
CREATE KEYSPACE IF NOT EXISTS LogAnalysis 
WITH replication = {'class':'SimpleStrategy', 'replication_factor':1};

CREATE TABLE IF NOT EXISTS LogAnalysis.NASALog (
    host text,
    time text,
    method text,
    url text,
    response text,
    bytes text,
    extension text,
    time_added text,
    PRIMARY KEY (host)
);
```

### 4. HDFS Setup
```bash
# Enter HDFS namenode container
docker exec -i -t namenode bash

# Create directory
hdfs dfs -mkdir -p /output/nasa_logs/
```

## 🎨 Visualization

![Real-time Dashboard](image/Realtime%20Dashboard.png)

The project includes a **multi-page Dash application** with:

### Dashboard Types
- **Real-time Dashboard**: Live streaming data visualization
- **Hourly Dashboard**: Hourly aggregated metrics
- **Daily Dashboard**: Daily trend analysis

### Available Visualizations
- **Response Code Distribution**: Pie charts showing HTTP response status distribution
- **Request Methods**: Analysis of GET, POST, PUT, DELETE requests
- **Traffic Patterns**: Time-series analysis of request volumes
- **Top Hosts**: Most active client hosts
- **URL Analysis**: Most requested resources