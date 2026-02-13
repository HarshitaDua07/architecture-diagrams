# Data Pipeline Architecture

## Real-Time & Batch Data Processing with Analytics

```mermaid
graph TB
    subgraph Sources["Data Sources"]
        WebApp["🌐 Web Application<br/>(Events)"]
        MobileApp["📱 Mobile Application"]
        API["🔌 Third-Party APIs"]
        Database["🗄️ Operational DB"]
        IoT["📡 IoT Devices"]
    end
    
    subgraph Ingestion["Data Ingestion Layer"]
        Kafka["📮 Apache Kafka<br/>(Event Streaming)"]
        Firehose["🚀 Kinesis Firehose<br/>(Real-time)"]
        Airflow["✈️ Apache Airflow<br/>(Orchestration)"]
        NiFi["🌊 Apache NiFi<br/>(Data Flow)"]
    end
    
    subgraph RealTimeProcessing["Real-Time Processing"]
        Spark["⚡ Apache Spark<br/>Streaming"]
        Flink["🌪️ Apache Flink"]
        KafkaStreams["🔄 Kafka Streams"]
        TimeSeries["📈 Time Series DB<br/>(InfluxDB)"]
    end
    
    subgraph BatchProcessing["Batch Processing"]
        SparkBatch["⚡ Spark Batch<br/>Processing"]
        Hadoop["📊 Hadoop<br/>MapReduce"]
        Hive["🐝 Hive<br/>SQL on Hadoop"]
    end
    
    subgraph Storage["Data Storage Layer"]
        DatalakeBronze["📦 Data Lake<br/>Bronze Layer<br/>(Raw Data)"]
        DatalakeSilver["🥈 Data Lake<br/>Silver Layer<br/>(Cleaned)"]
        DatalakeGold["🏆 Data Lake<br/>Gold Layer<br/>(Curated)"]
        DataWarehouse["🏪 Data Warehouse<br/>(Snowflake/Redshift)"]
    end
    
    subgraph DataQuality["Data Quality & Governance"]
        Validation["✅ Data Validation<br/>(Great Expectations)"]
        Lineage["🔗 Data Lineage<br/>(OpenLineage)"]
        Governance["📋 Data Governance<br/>(Collibra)"]
        Privacy["🔐 Privacy Manager<br/>(PII Detection)"]
    end
    
    subgraph Analytics["Analytics & BI"]
        Analytics["📊 Analytics Engine<br/>(Looker/Tableau)"]
        OLAP["🎲 OLAP Cube<br/>(Analysis)"]
        ML["🤖 ML Platform<br/>(MLflow)"]
        BI["📈 Business Intelligence<br/>Dashboard")
    end
    
    subgraph MachineLearning["Machine Learning Pipeline"]
        FeatureStore["🎁 Feature Store<br/>(Feast)"]
        Training["🧠 Model Training<br/>(TensorFlow/PyTorch)"]
        Inference["🔮 Inference Service<br/>(MLflow Serving)"]
        Monitoring["📊 Model Monitoring<br/>(Evidently AI)"]
    end
    
    subgraph Serving["Data Serving"]
        API_Serve["🔌 Data APIs<br/>(GraphQL)"]
        Cache["⚡ Cache Layer<br/>(Redis)"]
        WebUI["🖥️ Web Dashboard"]
    end
    
    WebApp -->|Stream| Kafka
    MobileApp -->|Stream| Kafka
    API -->|Pull| Firehose
    Database -->|ETL| Airflow
    IoT -->|Stream| Kafka
    
    Kafka -->|Real-time| Spark
    Firehose -->|Real-time| Flink
    Kafka -->|Stream| KafkaStreams
    Spark -->|Store| TimeSeries
    Flink -->|Store| TimeSeries
    
    Airflow -->|Orchestrate| SparkBatch
    Airflow -->|Orchestrate| Hadoop
    Hadoop -->|Query| Hive
    SparkBatch -->|Process| Hive
    
    Spark -->|Store| DatalakeBronze
    Flink -->|Store| DatalakeBronze
    SparkBatch -->|Store| DatalakeBronze
    
    DatalakeBronze -->|Clean| Validation
    Validation -->|Pass| DatalakeSilver
    DatalakeSilver -->|Aggregate| DatalakeGold
    DatalakeGold -->|Track| Lineage
    Lineage -->|Govern| Governance
    Governance -->|Apply| Privacy
    
    DatalakeGold -->|Load| DataWarehouse
    DataWarehouse -->|Query| Analytics
    DataWarehouse -->|OLAP| OLAP
    
    DatalakeGold -->|Features| FeatureStore
    FeatureStore -->|Train| Training
    Training -->|Deploy| Inference
    Inference -->|Monitor| Monitoring
    
    Analytics -->|Query| API_Serve
    OLAP -->|Query| API_Serve
    Inference -->|Serve| API_Serve
    API_Serve -->|Cache| Cache
    Cache -->|Display| WebUI
    Analytics -->|Display| BI
    
    Privacy -->|Audit| Governance
    
    style Sources fill:#ff6b6b
    style Ingestion fill:#4ecdc4
    style RealTimeProcessing fill:#95e1d3
    style BatchProcessing fill:#a8edea
    style Storage fill:#ffd93d
    style DataQuality fill:#f8b500
    style Analytics fill:#ff9999
    style MachineLearning fill:#c7b3e5
```

## Pipeline Layers

- **Data Sources**: Multiple data ingestion points (web, mobile, APIs, databases, IoT)
- **Ingestion Layer**: Kafka, Kinesis Firehose, Airflow, Apache NiFi
- **Real-Time Processing**: Spark Streaming, Flink, Kafka Streams
- **Batch Processing**: Spark, Hadoop, Hive
- **Data Storage**: Bronze → Silver → Gold data lake layers
- **Data Quality**: Validation, lineage, governance, privacy
- **Analytics**: BI tools, OLAP, ML platforms
- **ML Pipeline**: Feature store, model training, inference, monitoring
- **Data Serving**: APIs, caching, dashboards

## Architecture Highlights

- **Polyglot Ingestion**: Multiple data source connectors
- **Dual Processing**: Real-time and batch pipelines
- **Data Lake**: Bronze → Silver → Gold layers
- **Data Quality**: Validation and monitoring
- **ML Ready**: Feature store and model serving
- **Governance**: Lineage, privacy, and compliance