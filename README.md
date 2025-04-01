# 📈 Stock Market Data Pipeline using Apache Kafka, AWS S3, Glue & Athena

This project builds a real-time data pipeline for stock market data using **Apache Kafka** for data ingestion, **AWS S3** for storage, **AWS Glue** for cataloging, and **Amazon Athena** for analysis.

Kafka is installed and configured on an **AWS EC2 instance** to manage real-time data streaming for this project.

---

## 📦 Project Structure

```bash
├── KafkaProducer.ipynb     # Produces stock data to Kafka topic
├── KafkaConsumer.ipynb     # Consumes data from Kafka and uploads to S3
├── command_kafka.txt       # Kafka setup and command guide
├── README.md               # You're here!
```

---

## ⚙️ Setup Guide

### 1. Install and Start Kafka on EC2

```bash
wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz
tar -xvf kafka_2.12-3.3.1.tgz
cd kafka_2.12-3.3.1
```

### 2. Install Java

```bash
sudo yum install java-1.8.0-openjdk
java -version
```

### 3. Start Zookeeper

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

### 4. Start Kafka Broker (in a new session)

```bash
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
bin/kafka-server-start.sh config/server.properties
```

📌 **Update `server.properties`**  
Edit `ADVERTISED_LISTENERS` to match your EC2 public IP:

```bash
sudo nano config/server.properties
# Example:
advertised.listeners=PLAINTEXT://<EC2_PUBLIC_IP>:9092
```

---

## 🧵 Create Kafka Topic

```bash
bin/kafka-topics.sh --create --topic demo_testing2 \
--bootstrap-server <EC2_PUBLIC_IP>:9092 --replication-factor 1 --partitions 1
```

---

## 🧪 Kafka Producer

The Kafka Producer reads from a CSV stock dataset and pushes records to a Kafka topic.

**Run**: `KafkaProducer.ipynb`

---

## 📥 Kafka Consumer & S3 Upload

The Kafka Consumer listens to the topic, extracts messages, and uploads them as CSV files to an S3 bucket.

**Run**: `KafkaConsumer.ipynb`

---

## 🕵️ Glue Crawler and Athena

### 1. AWS Glue Crawler

- Go to AWS Glue
- Create a new crawler
- Set the S3 path to: `s3://your-s3-bucket/stock_data/`
- Run the crawler to catalog the data

### 2. Query with Amazon Athena

- Go to Athena
- Select the Glue database and table and perform the desired analysis by writing the queries.
---

## 🧱 Technologies Used

- Apache Kafka
- EC2 (Kafka Broker)
- AWS S3
- AWS Glue (Crawler)
- Amazon Athena
- Python (Pandas, Kafka-Python)

---

## 🚀 Future Improvements

- Add schema registry for better validation
- Use Spark Streaming for real-time transformation
- Add dashboards with Amazon QuickSight or Grafana
