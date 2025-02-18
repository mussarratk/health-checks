# health-checks
Scripts that help check the health of my computers.

This repo will be populated with lots of fancy checks.
## 1.Explore Data Concepts
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


| Feature | SQL Server on Azure VMs | Azure SQL Managed Instance | Azure SQL Database (Single Database) | Azure SQL Database (Elastic Pool) | Azure Database for MySQL | Azure Database for MariaDB | Azure Database for PostgreSQL |
|---|---|---|---|---|---|---|---|
| **Type of Cloud Service** | IaaS | PaaS | PaaS | PaaS | PaaS | PaaS | PaaS |
| **SQL Server Compatibility** | Fully compatible with on-premises SQL Server. "Lift and shift" migration. | Near 100% compatible. Minimal code changes for migration. | Supports most core database-level capabilities. Some features may be unavailable. | Supports most core database-level capabilities. Some features may be unavailable. | MySQL compatible | MariaDB compatible | PostgreSQL compatible |
| **Architecture** | SQL Server instances installed on a virtual machine. Multiple databases per instance. | Managed instance supports multiple databases. Instance pools for resource sharing. | Single database in a managed logical server. | Multiple databases share resources in an elastic pool. | MySQL server instance | MariaDB server instance | PostgreSQL server instance (flexible server, hyperscale option) |
| **Availability** | 99.99% | 99.99% | 99.995% | 99.995% | 99.99% | 99.99% | 99.99% |
| **Management** | Full management responsibility (OS, SQL Server updates, backups, etc.) | Automated updates, backups, and recovery. | Automated updates, backups, and recovery. | Automated updates, backups, and recovery. | Automated updates, backups, and recovery. | Automated updates, backups, and recovery. | Automated updates, backups, and recovery. |
| **Use Cases** | Migrate/extend on-premises SQL Server, retain full control. | Most cloud migration scenarios, minimal application changes. | New cloud solutions, minimal instance-level dependencies. | Variable workloads, cost optimization. | MySQL applications, LAMP stack, web apps. | MariaDB applications, Oracle compatibility, temporal data. | PostgreSQL applications, hybrid relational-object databases, geometric data. |
| **Control** | Highest level of control. | High level of control, some limitations compared to VMs. | Less control compared to VMs and Managed Instance. | Less control compared to VMs and Managed Instance. | Moderate control, some server parameters configurable. | Moderate control, some server parameters configurable. | High level of control, server configuration customizations |
| **Cost** | Varies based on VM size and SQL Server licensing. | Varies based on vCore and storage. | Varies based on DTU/vCore and storage. | Varies based on eDTU/vCore and storage. | Varies based on compute and storage. | Varies based on compute and storage. | Varies based on compute and storage. |
| **Scalability** | Manual VM resizing. | Scalable compute and storage. | Scalable compute and storage. | Scalable compute and storage. | Scalable compute and storage. | Scalable compute and storage. | Scalable compute and storage (Hyperscale: horizontal scaling) |
| **Features** | Full SQL Server feature set | Near-full SQL Server feature set | Core SQL Server features | Core SQL Server features | MySQL features (Community Edition based) | MariaDB features (Community Edition based) | PostgreSQL features (some extensions may be limited) |
| **Linked Servers** | Supported | Supported | Not supported (in some cases) | Not supported (in some cases) | N/A | N/A | N/A |
| **Service Broker** | Supported | Supported | Not supported | Not supported | N/A | N/A | N/A |
| **Database Mail** | Supported | Supported | Not supported | Not supported | N/A | N/A | N/A |
| **Hybrid Deployments** | Well-suited for hybrid scenarios. | Well-suited for hybrid scenarios. | Well-suited for hybrid scenarios. | Well-suited for hybrid scenarios. | Well-suited for hybrid scenarios. | Well-suited for hybrid scenarios. | Well-suited for hybrid scenarios. |
| **Migration Complexity** | Relatively straightforward "lift and shift". | Relatively straightforward, some compatibility checks recommended. | May require application changes due to feature limitations. | May require application changes due to feature limitations. | Data migration tools available (DMS). | Data migration tools available (DMS). | Data migration tools available (DMS). |
| **Best For** | Existing applications requiring full SQL Server functionality and OS access. | Migrating existing SQL Server instances with minimal changes. | New cloud applications with minimal dependencies and cost optimization. | Applications with fluctuating workloads and cost optimization. | MySQL-based applications, LAMP stack, web apps. | MariaDB-based applications, Oracle compatibility, temporal data. | PostgreSQL-based applications, hybrid relational-object databases, geometric data. |
| **Instance Scope** | Instance-level | Instance-level | Database-level | Database-level | Instance-level | Instance-level | Instance-level |
| **Pricing Model** | Pay-as-you-go (VM costs + SQL Server license) | Pay-as-you-go (vCores + storage) | Pay-as-you-go (DTUs/vCores + storage) | Pay-as-you-go (eDTUs/vCores + storage) | Pay-as-you-go (compute + storage) | Pay-as-you-go (compute + storage) | Pay-as-you-go (compute + storage) |
| **Open Source** | No | No | No | No | Yes (Community Edition based) | Yes (Community Edition based) | Yes |
| **Key Features/Specializations** | Full SQL Server feature set | Near-full SQL Server feature set | Core SQL Server features | Core SQL Server features | Leading open-source RDBMS, LAMP stack, various editions (Community, Standard, Enterprise) | Created by original MySQL developers, optimized performance, temporal data support, Oracle compatibility | Hybrid relational-object database, custom data types, extensible, geometric data support, pgsql query language |
| **pgAdmin Support (PostgreSQL)** | N/A | N/A | N/A | N/A | N/A | N/A | Yes (for managing databases, not server) |
| **Query Store (PostgreSQL)** | N/A | N/A | N/A | N/A | N/A | N/A | Yes (azure_sys database, query_store.qs_view) |
| **Provisioning Tools** | Azure Portal, Azure CLI, Azure PowerShell, ARM Templates | Azure Portal, Azure CLI, Azure PowerShell, ARM Templates | Azure Portal, Azure CLI, Azure PowerShell, ARM Templates | Azure Portal, Azure CLI, Azure PowerShell, ARM Templates | Azure Portal, Azure CLI, Azure PowerShell, ARM Templates | Azure Portal, Azure CLI, Azure PowerShell, ARM Templates | Azure Portal, Azure CLI, Azure PowerShell, ARM Templates |
| **Scaling** | Manual VM resizing | Scalable compute and storage | Scalable compute and storage | Scalable compute and storage | Scalable compute and storage | Scalable compute and storage | Scalable compute and storage (Hyperscale: horizontal scaling) |
| **Provisioning Process (MySQL/PostgreSQL)** | Via Azure Portal (Marketplace), similar steps for both | Via Azure Portal (Marketplace) | Via Azure Portal (Marketplace) | Via Azure Portal (Marketplace) | Via Azure Portal (Marketplace) | Via Azure Portal (Marketplace) | Via Azure Portal (Marketplace) |
| **Hyperscale Option** | N/A | N/A | N/A | N/A | N/A | N/A | Yes (horizontal scaling, query parallelization, multi-tenant apps, real-time analytics) |
| **Provisioning Details (MySQL/PostgreSQL)** | Subscription, Resource Group, Server Name, Backup (optional), Location, Version, Compute & Storage (Pricing Tier: Basic, General Purpose, Memory Optimized), Admin Username, Password | Subscription, Resource Group, Server Name, Backup (optional), Location, Version, Compute & Storage (Pricing Tier: Basic, General Purpose, Memory Optimized), Admin Username, Password | Subscription, Resource Group, Server Name, Backup (optional), Location, Version, Compute & Storage (Pricing Tier: Basic, General Purpose, Memory Optimized), Admin Username, Password | Subscription, Resource Group, Server Name, Backup (optional), Location, Version, Compute & Storage (Pricing Tier: Basic, General Purpose, Memory Optimized), Admin Username, Password | Subscription, Resource Group, Server Name, Backup (optional), Location, Version, Compute & Storage (Pricing Tier: Basic, General Purpose, Memory Optimized), Admin Username, Password | Subscription, Resource Group, Server Name, Backup (optional), Location, Version, Compute & Storage (Pricing Tier: Basic, General Purpose, Memory Optimized), Admin Username, Password | Subscription, Resource Group, Server Name, Backup (optional), Location, Version, Compute & Storage (Pricing Tier: Basic, General Purpose, Memory Optimized), Admin Username, Password |
| **Compute & Storage Tiers (MySQL/PostgreSQL)** | Basic, General Purpose, Memory Optimized | Basic, General Purpose, Memory Optimized | Basic, General Purpose, Memory Optimized | Basic, General Purpose, Memory Optimized | Basic, General Purpose, Memory Optimized | Basic, General Purpose, Memory Optimized | Basic, General Purpose, Memory Optimized |
| **Admin Username Restrictions (MySQL/PostgreSQL)** | Cannot be Azure








