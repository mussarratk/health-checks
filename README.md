# health-checks
Scripts that help check the health of my computers.

This repo will be populated with lots of fancy checks.

## Comparison of Data Storage Formats

| Format | Type | Storage Structure | Schema Handling | Compression | Use Cases | Strengths | Weaknesses | Example (Simplified) |
|---|---|---|---|---|---|---|---|---|
| **Avro** | Binary, Row-based | Records with headers | Schema defined in JSON, supports schema evolution | Excellent (various codecs) | Data serialization, RPC, message queues (Kafka) | Efficient storage and transmission, schema evolution | Less efficient for analytical queries needing only a few columns | See previous response |
| **ORC (Optimized Row Columnar)** | Binary, Columnar | Stripes (columns within stripes) | Schema defined | Good (various codecs) | Hive workloads, data warehousing | Efficient for analytical queries, footer statistics for query optimization | Primarily used within the Hadoop ecosystem | See previous response |
| **Parquet** | Binary, Columnar | Row groups (columns within row groups), chunks | Schema defined | Excellent (various codecs) | Data lakes, analytical applications, Spark, Hadoop | Efficient for analytical queries, handles nested data well, wide ecosystem support | Can be less efficient for row-oriented operations | See previous response |
| **JSON (JavaScript Object Notation)** | Text-based, Row-based (typically) | Key-value pairs, nested objects and arrays | Schema optional (self-describing), can be defined externally (e.g., JSON Schema) | Limited (usually relies on external compression) | Web APIs, configuration files, NoSQL databases | Human-readable, easy to parse in many languages | Verbose, less efficient for large datasets, limited data types | `{"id": 1, "name": "Alice", "favorite_colors": ["red", "blue"]}` |
| **XML (Extensible Markup Language)** | Text-based, Hierarchical | Tags, attributes, nested elements | Schema defined (DTD, XSD) | Limited (usually relies on external compression) | Configuration files, data exchange between systems | Human-readable, supports complex data structures | Verbose, less efficient than binary formats, parsing can be complex | `<user><id>1</id><name>Alice</name><favorite_colors><color>red</color><color>blue</color></favorite_colors></user>` |
| **Blob (Binary Large Object)** | Binary | Unstructured sequence of bytes | No inherent schema | Varies (depends on content) | Storing images, videos, documents, other unstructured data | Can store any type of data | No built-in structure or metadata, requires application to interpret | (Cannot be meaningfully represented in a simple text example - think of a raw image file) |
| **CSV (Comma Separated Values)** | Text-based, Row-based | Comma-separated values in rows | No inherent schema (often inferred) | Limited (usually relies on external compression) | Simple data exchange, spreadsheets | Human-readable, easy to generate | Limited data types, no support for nested structures, schema management can be problematic | `1,Alice,red|blue` (pipe as color separator) |

**Key Considerations:**

*   **Schema:**  Avro, ORC, and Parquet have robust schema handling, which is crucial for data consistency and efficient processing.  JSON and XML can have schemas, but they are often optional or less enforced. Blobs and CSV typically do not have inherent schemas.
*   **Performance:** Binary formats (Avro, ORC, Parquet) are generally much more efficient for storage and processing large datasets than text-based formats (JSON, XML, CSV).
*   **Use Cases:** The choice of format depends heavily on the specific application.  Avro is good for serialization, ORC for Hive, Parquet for general-purpose analytics, JSON for web APIs, XML for complex data exchange, Blobs for unstructured data, and CSV for simple data exchange.
*   **Compression:** Binary formats often support efficient compression, reducing storage space and I/O overhead. Text-based formats usually require external compression tools.
*   **Ecosystem:**  The availability of tools and libraries for working with a particular format is also an important factor.  Parquet, JSON, and CSV have very wide ecosystem support.

This table provides a more comprehensive overview of the different data storage formats, including their strengths, weaknesses, and typical use cases.  Remember that the "best" format depends on your specific needs.

## Analytical Data Processing Architectures: A Comparison

| Component/Feature | Data Lake | Data Warehouse | Data Lakehouse | OLAP Model (Cube) |
|---|---|---|---|---|
| **Data Storage** | File-based (raw, semi-structured, structured) | Relational tables (structured) | File-based with tabular abstractions (structured & unstructured) | Pre-aggregated relational (structured) |
| **Data Processing** | ETL, Data Science workloads (Spark, etc.) | ETL, SQL queries | ETL, Data Science & SQL workloads (Spark, Delta Lake, etc.) | Aggregation, pre-calculation |
| **Schema** | Flexible, schema-on-read | Fixed, schema-on-write | Flexible with schema enforcement capabilities | Fixed, pre-defined aggregations |
| **Scalability** | Highly scalable | Scalable, but less flexible than data lakes | Highly scalable and flexible | Scalable, optimized for specific queries |
| **Performance** | Varies depending on data format and processing engine | Optimized for read queries | Combines the performance of data warehouses with the flexibility of data lakes | Extremely fast for pre-defined aggregations |
| **Data Types** | Handles diverse data types (raw, semi-structured, structured) | Primarily structured data | Handles diverse data types (raw, semi-structured, structured) | Primarily structured, aggregated data |
| **User Roles** | Data Scientists, Data Engineers | Data Analysts, Business Analysts | Data Scientists, Data Engineers, Data Analysts | Business Users, Analysts |
| **Use Cases** | Exploration, machine learning, data discovery | Reporting, dashboards, business intelligence | Combining exploration and reporting, advanced analytics | Fast reporting, multidimensional analysis, drill-down capabilities |
| **Example Technologies** | Hadoop, Spark, Cloud Storage (AWS S3, Azure Blob Storage) | SQL Databases (PostgreSQL, Snowflake, BigQuery) | Delta Lake, Apache Hudi, Iceberg | MDX, OLAP servers (e.g., Microsoft Analysis Services) |


## Analytical vs. Transactional Data Processing: A Comparison

| Feature | Analytical Data Processing (OLAP) | Transactional Data Processing (OLTP) |
|---|---|---|
| **Purpose** | Analyze historical data, identify trends, support decision-making | Record transactions, support business operations |
| **Data Characteristics** | Large volumes of historical data, often aggregated | High volume of current data, individual transactions |
| **Data Operations** | Read-heavy, complex queries, aggregations | Read and write-heavy, simple, frequent operations (CRUD) |
| **Schema** | Flexible (Data Lake, Data Lakehouse) or optimized for queries (Data Warehouse, OLAP Cube) | Fixed, normalized, optimized for transactions |
| **Performance** | Optimized for complex queries, reporting, and analysis | Optimized for fast, reliable transaction processing |
| **Scalability** | Scalable for large datasets and complex analysis | Scalable for high transaction volumes and concurrency |
| **Data Consistency** | Important, but some latency may be acceptable | Critical, ACID properties enforced |
| **Data Integrity** | Important, but some data quality issues may be tolerated | Critical, data integrity is paramount |
| **Concurrency** | Lower concurrency requirements | High concurrency requirements, many users/applications accessing data simultaneously |
| **Typical Users** | Data Scientists, Data Analysts, Business Users | Operational Staff, Customers (via applications) |
| **System Focus** | Data exploration, reporting, visualization, business intelligence | Transaction logging, order processing, inventory management, financial transactions |
| **Example Technologies** | Hadoop, Spark, Cloud Storage, SQL Databases, OLAP Servers | SQL Databases (MySQL, PostgreSQL, Oracle), NoSQL Databases, Mainframes |
| **Key Concepts** | Data Lake, Data Warehouse, Data Lakehouse, OLAP Cube, ETL | Transactions, ACID properties, CRUD operations, Normalization |
