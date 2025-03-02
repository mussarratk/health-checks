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

## Relational Database

| Feature | (SQL Server) on Azure VMs | Azure (SQL Server) Managed Instance | Azure (SQL Server) Database (Single Database) | Azure (SQL Server) Database (Elastic Pool) | Azure Database for MySQL | Azure Database for MariaDB | Azure Database for PostgreSQL |
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


# Azure SQL and Database Services Comparison

| Feature | **SQL Server on Azure VMs (Full Features)** | **Azure SQL Managed Instance** | **Azure SQL Database** | **Azure Database for MySQL** | **Azure Database for MariaDB** | **Azure Database for PostgreSQL** |
|---------|--------------------------------------------|-------------------------------|-------------------------|------------------------------|--------------------------------|--------------------------------|
| **Type** | Infrastructure-as-a-Service (IaaS) | Platform-as-a-Service (PaaS) | Platform-as-a-Service (PaaS) | Platform-as-a-Service (PaaS) | Platform-as-a-Service (PaaS) | Platform-as-a-Service (PaaS) |
| **Features** | Full SQL Server feature set (all editions) | Near-full SQL Server feature set. Some limitations (e.g., linked servers in some cases). | Core SQL Server features. Some limitations (e.g., linked servers, Service Broker, Database Mail). | MySQL features (Community Edition based). Some extensions may be limited. | MariaDB features (Community Edition based). Some extensions may be limited. | PostgreSQL features. Some extensions may be limited. |
| **Linked Servers** | ✅ Fully supported | ✅ Limited support | ❌ Not supported | ❌ Not applicable | ❌ Not applicable | ✅ Supported but may require configuration |
| **Service Broker** | ✅ Supported | ✅ Supported | ❌ Not supported | ❌ Not applicable | ❌ Not applicable | ❌ Not applicable |
| **Database Mail** | ✅ Supported | ✅ Supported | ❌ Not supported | ❌ Not applicable | ❌ Not applicable | ❌ Not applicable |
| **SQL Server Agent** | ✅ Fully supported | ✅ Fully supported | ❌ Not supported (alternative: Elastic Jobs) | ❌ Not applicable | ❌ Not applicable | ❌ Not applicable |
| **SSIS (Integration Services)** | ✅ Fully supported | ✅ Limited support | ❌ Not supported (Azure Data Factory as an alternative) | ❌ Not applicable | ❌ Not applicable | ❌ Not applicable |
| **SSAS (Analysis Services)** | ✅ Fully supported | ❌ Not supported | ❌ Not supported (Use Azure Analysis Services instead) | ❌ Not applicable | ❌ Not applicable | ❌ Not applicable |
| **SSRS (Reporting Services)** | ✅ Fully supported | ❌ Not supported | ❌ Not supported (Use Power BI or other cloud-based reporting solutions) | ❌ Not applicable | ❌ Not applicable | ❌ Not applicable |
| **Full-Text Search** | ✅ Fully supported | ✅ Supported | ✅ Supported | ✅ Supported | ✅ Supported | ✅ Supported |
| **Change Data Capture (CDC) & Change Tracking** | ✅ Fully supported | ✅ Supported | ✅ Supported (with limitations) | ✅ Supported (via binlog replication) | ✅ Supported (via binlog replication) | ✅ Supported (logical replication, wal_level setting) |
| **Advanced Security** | ✅ Full control over encryption, auditing, fine-grained permissions | ✅ Limited control, but many security features | ✅ Security managed by Microsoft with limited customization | ✅ Security managed by Microsoft with limited customization | ✅ Security managed by Microsoft with limited customization | ✅ Security managed by Microsoft with limited customization |
| **Operating System Access** | ✅ Full access to OS (Windows Server/Linux) | ❌ No OS-level access | ❌ No OS-level access | ❌ No OS-level access | ❌ No OS-level access | ❌ No OS-level access |
| **Administrative Control** | ✅ Complete administrative rights, including instance-level settings, backup strategies, tuning, and scaling | ✅ Partial administrative control (instance-level) | ❌ Limited administrative control (database-level only) | ❌ Limited administrative control (database-level only) | ❌ Limited administrative control (database-level only) | ❌ Limited administrative control (database-level only) |
| **Management Overhead** | ❌ High – You must manage OS, SQL patches, backups, DR, performance, scaling, security | ✅ Medium – Microsoft manages most admin tasks, but some instance-level management is required | ✅ Low – Microsoft automates maintenance, patching, backups, and monitoring | ✅ Low – Microsoft automates maintenance, patching, backups, and monitoring | ✅ Low – Microsoft automates maintenance, patching, backups, and monitoring | ✅ Low – Microsoft automates maintenance, patching, backups, and monitoring |
| **Scalability** | ✅ Manual scaling of VM size, storage, and compute resources | ✅ Auto-scaling with flexible instance sizes | ✅ Auto-scaling, including serverless compute options | ✅ Auto-scaling | ✅ Auto-scaling | ✅ Auto-scaling |
| **Application Compatibility** | ✅ Best for lift-and-shift migrations from on-prem | ✅ Good for most SQL Server workloads | ❌ Some applications may require modifications due to feature limitations | ✅ Compatible with MySQL applications | ✅ Compatible with MariaDB applications | ✅ Compatible with PostgreSQL applications |
| **Disaster Recovery & Backups** | ❌ You must configure and manage backups and high availability (e.g., Always On Availability Groups) | ✅ Automatic backups, geo-replication, high availability | ✅ Automatic backups, geo-replication, high availability | ✅ Automatic backups, geo-replication, high availability | ✅ Automatic backups, geo-replication, high availability | ✅ Automatic backups, geo-replication, high availability |
| **Compliance & Governance** | ✅ Full control over configurations, security policies, and compliance standards | ✅ Microsoft ensures compliance, but customization is limited | ✅ Microsoft ensures compliance with various industry regulations, but customization is limited | ✅ Microsoft ensures compliance with various industry regulations, but customization is limited | ✅ Microsoft ensures compliance with various industry regulations, but customization is limited | ✅ Microsoft ensures compliance with various industry regulations, but customization is limited |
| **Performance Tuning** | ✅ Complete control over indexing, query optimization, and performance tuning | ✅ Manual tuning possible, but Microsoft provides some optimizations | ✅ Microsoft optimizes performance with AI-driven tuning, but manual control is limited | ✅ Some manual tuning possible, but limited compared to on-prem | ✅ Some manual tuning possible, but limited compared to on-prem | ✅ Some manual tuning possible, but limited compared to on-prem |
| **Monitoring & Diagnostics** | ✅ Requires third-party or custom monitoring solutions | ✅ Built-in monitoring and alerting tools | ✅ Built-in monitoring, alerts, and automated performance tuning | ✅ Built-in monitoring tools | ✅ Built-in monitoring tools | ✅ Built-in monitoring tools |
| **Cost** | ❌ Higher – Pay for VM resources (compute, storage, networking) + SQL Server license (unless using Software Assurance) | ✅ Medium – Higher than Azure SQL Database but lower than VMs | ✅ Lower – Pay for compute/storage only, with serverless options for cost efficiency | ✅ Lower – Pay for compute/storage only | ✅ Lower – Pay for compute/storage only | ✅ Lower – Pay for compute/storage only |
| **Best Use Cases** | - Applications requiring full SQL Server capabilities | - Lift-and-shift migrations | - Custom configurations & compliance needs | - Large-scale enterprise databases | - Applications requiring deep OS-level integration | - Existing SQL Server apps needing managed instance support | - Partial lift-and-shift scenarios | - Multi-database deployments with near-full SQL Server functionality | - Web and cloud-native applications | - Scalable SaaS applications | - Cost-sensitive workloads | - Minimal administrative effort required | - Short-term or variable workloads needing serverless | - Web applications using MySQL | - WordPress and CMS-based applications | - Open-source projects that need MySQL | - Web applications using MariaDB | - CMS and e-commerce platforms | - Open-source projects requiring MariaDB | - Applications requiring PostgreSQL features | - Advanced analytics workloads | - Geographic distribution and replication |

## Conclusion
- **Choose SQL Server on Azure VMs** if you need complete control and all SQL Server functionalities.
- **Choose Azure SQL Managed Instance** if you want near-full SQL Server features in a managed PaaS solution.
- **Choose Azure SQL Database** for a cost-effective, fully managed SQL Server option with reduced administrative overhead.
- **Choose Azure Database for MySQL/MariaDB/PostgreSQL** if you need an open-source relational database with automatic scaling and management.

# Azure SQL and Database Offerings - Comparative Overview

## Provisioning Details
| Feature                           | SQL Server on Azure VMs | Azure SQL Managed Instance | Azure SQL Database (Single Database) | Azure SQL Database (Elastic Pool) | Azure Database for MySQL | Azure Database for PostgreSQL | Azure Database for MariaDB |
|-----------------------------------|-------------------------|----------------------------|--------------------------------------|----------------------------------|---------------------------|---------------------------|---------------------------|
| Subscription & Resource Group     | ✅ Yes                   | ✅ Yes                      | ✅ Yes                                | ✅ Yes                            | ✅ Yes                    | ✅ Yes                    | ✅ Yes                    |
| Compute & Storage                 | Various VM sizes & SQL Server editions | Various vCore & storage options | Basic, Standard, Premium, Serverless | Various eDTU/vCore & storage options | Burstable, General Purpose, Memory Optimized | Burstable, General Purpose, Memory Optimized | Burstable, General Purpose, Memory Optimized |

## Security & Network
| Feature                           | SQL Server on Azure VMs | Azure SQL Managed Instance | Azure SQL Database (Single Database) | Azure SQL Database (Elastic Pool) | Azure Database for MySQL | Azure Database for PostgreSQL | Azure Database for MariaDB |
|-----------------------------------|-------------------------|----------------------------|--------------------------------------|----------------------------------|---------------------------|---------------------------|---------------------------|
| Admin Username Restrictions       | N/A                     | N/A                        | Cannot use Azure, superuser, admin, etc. | Cannot use Azure, superuser, admin, etc. | Cannot use Azure, superuser, admin, etc. | Cannot use Azure, superuser, admin, etc. | Cannot use Azure, superuser, admin, etc. |
| Network Security                  | VLANs, ACLs, Firewalls  | NSGs, Firewalls           | Firewalls, connection policies      | Firewalls, connection policies   | NSGs, Firewalls          | NSGs, Firewalls          | NSGs, Firewalls          |
| Connection Policy                  | Redirect or Proxy      | Redirect or Proxy         | Redirect or Proxy                   | Redirect or Proxy                | Redirect or Proxy        | Redirect or Proxy        | Redirect or Proxy        |
| Default Connection Port           | 1433                    | 1433                       | 1433                                 | 1433                             | 3306                      | 5432                      | 3306                      |
| DoS Protection                    | ✅ DoSGuard             | ✅ DoSGuard                | ✅ DoSGuard                          | ✅ DoSGuard                      | ✅ DoSGuard               | ✅ DoSGuard               | ✅ DoSGuard               |
| SSL Connection                    | Configurable           | Configurable              | Configurable                        | Configurable                     | Required                 | Required                 | Required                 |

## Performance & Features
| Feature                           | SQL Server on Azure VMs | Azure SQL Managed Instance | Azure SQL Database (Single Database) | Azure SQL Database (Elastic Pool) | Azure Database for MySQL | Azure Database for PostgreSQL | Azure Database for MariaDB |
|-----------------------------------|-------------------------|----------------------------|--------------------------------------|----------------------------------|---------------------------|---------------------------|---------------------------|
| Linked Server Support             | ✅ Fully Supported      | ✅ Supported               | ❌ Limited                          | ❌ Limited                      | ❌ Not Supported          | ❌ Not Supported          | ❌ Not Supported          |
| Performance                        | High (depends on VM size) | High (near full SQL Server compatibility) | Medium (depends on DTU/vCores) | Medium (depends on eDTU/vCores) | Medium | Medium | Medium |
| Security Control                   | Full control over security setup | Managed security with some control | Managed security with limited control | Managed security with limited control | Managed security with limited control | Managed security with limited control | Managed security with limited control |

## Summary
- **SQL Server on Azure VMs** provides full control and supports all features, including Linked Servers.
- **Azure SQL Managed Instance** offers near-full SQL Server compatibility with a managed environment.
- **Azure SQL Database (Single & Elastic Pool)** is best for cloud-native applications but has limitations with Linked Servers.
- **Azure Database for MySQL, PostgreSQL, and MariaDB** are fully managed but do not support Linked Servers.

🚀 Choose the right database service based on your workload, security, and integration needs.
![image](https://github.com/user-attachments/assets/347957fe-e518-4dba-909e-f81d0031b1ef) 
- **PaaS Appracg - Azure SQL Database is a hassle-free way to run SQL Server in the cloud. Microsoft takes care of all the technical stuff, so you can focus on your data. It is very good for modern cloud applications.
- Azure SQL Database is available with Single Database, Elastic Pool, and Managed Instance deployments. 
- A hybrid deployment is a system where part of the operation runs on-premises, and part in the cloud. Your database might be part of a larger system that runs on-premises, although the database elements might be hosted in the cloud.
- Single Database and Elastic Pool options restrict some of the administrative features available to SQL Server. Managed instance effectively runs a fully controllable instance of SQL Server in the cloud.
- Azure SQL Managed Instance employs a layered security approach, using encryption, digital signatures, and certificate verification to protect data. The continuous monitoring of CRLs ensures that only trusted parties can communicate with the Managed Instance, mitigating the risk of security breaches.
- Not all features of a database management system are available in Azure Data Services. This is because Azure Data Services takes on the task of managing the system and keeping it running using hardware situated in an Azure datacenter. Azure Data Services offer a compelling trade-off: reduced management and high availability in exchange for some limitations in customization and control.The suitability of Azure Data Services depends on your specific needs and priorities. If you require maximum control and compatibility, running SQL Server on Azure VMs (IaaS) might be a better option. If you prioritize ease of use and reduced management, Azure Data Services (PaaS) are an excellent choice.
- The DBMS handles the physical aspects of a database, such as where and how it's stored, who can access it, and how to ensure that it's available when required. You get the core DBMS functionalities you need, but Azure takes care of the underlying infrastructure and management. This allows you to focus on your data and applications, while Azure ensures the reliability and security of your database.
- PaaS offerings like Azure SQL Database prioritize simplicity and automation, but they abstract away OS access.
IaaS offerings like SQL virtual machines provide maximum control and compatibility, making them ideal for migrating existing applications that rely on OS-level functionalities.
When you need to move a database solution to the cloud quickly, and with the least amount of disruption, IaaS is the best option.
- Azure Database for MySQL High availability. Easy scaling that responds quickly to demand. Secure data, both at rest and in motion. Automatic backups and point-in-time restore for the last 35 days.
- Azure PostgreSQL Hyperscale (Citus) is a deployment option that scales queries across multiple server nodes to support large database loads. Your database is split across nodes.

  ![image](https://github.com/user-attachments/assets/2bb42a7d-d80d-4487-b113-4c1f222784c2)
![image](https://github.com/user-attachments/assets/3b5425c3-0421-47f4-a225-757ca2055396)
![image](https://github.com/user-attachments/assets/36ffbbbf-2bf8-4072-b099-ba3066e152b1)
![image](https://github.com/user-attachments/assets/b5a60c85-ba63-42d8-b4b1-bc0843ae9fd2)

https://learn.microsoft.com/en-us/training/modules/query-relational-data/6-exercise-perform-query?wt.mc_id=azsql_ex6prfmquery_webpage_extlp
![image](https://github.com/user-attachments/assets/effcb1b1-ca4f-4527-a461-0ae75b10c260)

# Non-Relational

# Azure Storage Services Hierarchy and Details

This document outlines the hierarchy and key features of various Azure Storage services, presented in a collapsible markdown format.

## 1. Core Storage Concepts

*   **Storage Account:** The fundamental unit of management and billing for Azure Storage.  All Azure Storage services reside within a storage account. Provides a unique namespace for your Azure Storage data. Required for Blob, File, and Table storage.
*   **Containers/File Shares/Tables/Queues/Cosmos DB Containers:** Logical groupings of data within a storage account. The specific type depends on the storage service being used.
*   **Blobs/Files/Entities/Messages/Documents:** The actual data objects stored within the containers/file shares/tables/queues/Cosmos DB containers.

## 2. Azure Blob Storage (Unstructured Data)

*   **Purpose:** Stores massive amounts of unstructured data (binary large objects). Ideal for binary large objects (BLOBs).
*   **Key Features:**
    *   **Blob Types:**
        *   **Block Blobs:** Sets of blocks (up to 100 MB each, max 50,000 blocks, 4.7 TB max). Best for discrete, large, infrequently changing binary objects.
        *   **Page Blobs:** Collections of 512-byte pages (up to 8 TB). Optimized for random read/write operations (used for VM disks).
        *   **Append Blobs:** Block blobs optimized for append operations (blocks up to 4 MB, max 195 GB). Only append operations allowed.
    *   **Containers:** Logical grouping of blobs within a storage account. Supports folder-like hierarchy. Container level access control. (Storage Account -> Containers -> Folders)
    *   **Access Tiers:**
        *   **Hot:** Frequently accessed data (high performance, higher cost). Milliseconds typical read latency.
        *   **Cool:** Infrequently accessed data (lower performance, reduced cost). Milliseconds typical read latency.
        *   **Archive:** Rarely accessed historical data (lowest cost, high latency). Hours to rehydrate.
    *   **Access Tier Transitions:** Moving blobs between tiers. Rehydration is required for Archive.
    *   **Lifecycle Management Policies:** Automated management of blob lifecycle.
    *   **Redundancy:** LRS, GRS.
    *   **Versioning:** Maintaining and restoring previous blob versions.
    *   **Soft Delete:** Recovering accidentally deleted blobs.
    *   **Snapshots:** Read-only versions of a blob at a specific point in time.
    *   **Change Feed:** Ordered record of updates made to a blob.
    *   **Block Blob Maximum Size:** 4.7 Terabytes (50,000 blocks x 100 MB).
    *   **Page Blob Maximum Size:** 8 Terabytes.
    *   **Append Blob Maximum Size:** 195 Gigabytes.
    *   **Block Blob Block Size:** 100 Megabytes.
    *   **Append Blob Block Size:** 4 Megabytes.
    *   **Page Blob Page Size:** 512 Bytes.
*   **Use Cases:** Serving images/documents, static website hosting, streaming, backup/restore, archiving, data analysis.

## 3. Azure Files (Cloud-Based File Shares)

*   **Purpose:** Provides cloud-based network file shares, accessible via SMB 3.0.
*   **Key Features:**
    *   **Access Protocol:** SMB 3.0.
    *   **Authentication:** Azure Active Directory Domain Services.
    *   **Max Share Size:** 100 Terabytes (per storage account).
    *   **Max File Size:** 1 Terabyte.
    *   **Concurrent Connections:** 2,000 (per shared file).
    *   **Upload Methods:** Azure Portal, AzCopy, Azure File Sync.
    *   **Performance Tiers:** Standard (HDD), Premium (SSD).
    *   **Data Replication:** Locally and Geo-redundant.
    *   **Throughput:** Up to 300 MB/s (Standard). Premium provides higher throughput.
    *   **Data Encryption:** At rest and in transit.
*   **Use Cases:** Application migration, server data sharing, modern application integration, high availability workloads.
*   **Important Considerations:** Not ideal for multiple concurrent writers without synchronization. Potential data overwrite issues without proper file locking.

## 4. Azure Table Storage (NoSQL Key-Value Store)

*   **Purpose:** Stores semi-structured, denormalized data in a NoSQL key-value format.
*   **Key Features:**
    *   **Data Model:** Key-value model, denormalized.
    *   **Partitions:** Tables are split into partitions.
    *   **Data Replication:** 3 times within an Azure region.
*   **Advantages:** Flexible, semi-structured data, no complex relationship mapping.
*   **Limitations:** Lacks relational database features.
*   **Use Cases:** User data, configuration information, log data.

## 5. Microsoft OneLake (Unified Data Lake)

*   **Foundation:** Built on Azure Data Lake Storage Gen2.
*   **Purpose:** Provides a single, unified, logical data lake for an entire organization.
*   **Key Features:**
    *   **Organization-wide Data Lake:** Centralized repository for all analytics data.
    *   **Distributed Ownership:** Workspaces enable team management while maintaining governance.
    *   **Open and Compatible:** Uses Delta Parquet format, supports ADLS Gen2 APIs.
    *   **Easy Navigation:** OneLake file explorer for Windows.
    *   **Shortcuts:** Virtualization of data across domains without duplication.
    *   **Security:** Microsoft Entra ID integration.
    *   **Integration with Fabric Workloads:** Seamless integration with all Fabric services.
*   **Relationship to ADLS Gen2:** OneLake is a higher-level abstraction that simplifies the use of ADLS Gen2.  It leverages the underlying storage capabilities of ADLS Gen2.

## 6. Azure Data Lake Storage Gen2 (ADLS Gen2)

*   **Purpose:**  A massively scalable and secure data lake service for big data analytics.  Combines the power of Azure Blob Storage with the hierarchical namespace of Azure Files.
*   **Key Features:**
    *   **Hierarchical Namespace:** Organizes data into directories and subdirectories, improving manageability and performance.
    *   **POSIX Permissions:** Supports access control lists (ACLs) for fine-grained permissions.
    *   **Scalability and Performance:** Optimized for large-scale data processing.
    *   **Integration with Azure Ecosystem:** Works seamlessly with other Azure services.
*   **Note:**  ADLS Gen2 is the underlying storage for OneLake.

## 7. Azure Cosmos DB (Multi-Model NoSQL Database)

*   **Purpose:** Multi-model NoSQL database. Manages data as partitioned sets of documents.
*   **Key Features:**
    *   **Data Model:** Document-based (JSON-like).
    *   **Document Structure:** Flexible, schema-free.
    *   **Document Size Limit:** 2 Megabytes (including small binary objects).
    *   **APIs:** SQL, Table, MongoDB, Cassandra, Gremlin.
    *   **Containers:** Logical grouping of documents.
    *   **Partitions:** Group documents with a common partition key.
    *   **Indexing:** Automatic indexing of all fields.
    *   **Scalability:** Highly scalable, automatic partition management.
    *   **Availability:** 99.99% high availability.
    *   **Consistency Levels:** Five well-defined consistency levels.
    *   **Latency:** Less than 10ms for reads and writes (99th percentile).
    *   **Security:** Encryption at rest and in transit, row-level authorization.
*   **Use Cases:** IoT, e-commerce, gaming, web and mobile applications.

This comprehensive structure now incorporates the additional details you provided, offering a more in-depth view of each Azure Storage service.  Remember that the collapsible sections allow you to expand and collapse information as needed.

## Benefits of embedding:

* Improved read performance: When you retrieve the customer object, you get all their order information in a single operation. ** No need to make separate requests to fetch the orders. This significantly speeds up data retrieval.
** Simplified queries: You don't need to perform joins or lookups to combine data from multiple sources.
* Drawbacks of embedding: Data redundancy: If the same order information is needed in other contexts (e.g., displaying all orders), it will be duplicated in every customer object that has that order.
** Increased storage space: Storing the same data multiple times consumes more storage.
** Update complexity: If an order is modified, you need to update it in every customer object where it's embedded. This can be challenging to maintain data consistency.
  - Embedding in JSON is a powerful technique for denormalizing data and improving read performance. It involves nesting related data directly within a JSON object.  However, it's essential to consider the trade-offs in terms of data redundancy, storage space, and update complexity before using it.  It's a common and effective strategy in NoSQL databases where optimizing for read speed is often a priority.
![image](https://github.com/user-attachments/assets/5f4b2569-6612-4f1b-a495-477eff081739)
![image](https://github.com/user-attachments/assets/7e52c044-8779-4722-895e-1b940e72728c)
![image](https://github.com/user-attachments/assets/1c830ad7-3214-48be-b95a-15f078b1fe62)
![image](https://github.com/user-attachments/assets/a54fa23e-1175-4dfb-afb7-6815ca7e5937)
![image](https://github.com/user-attachments/assets/874a2c42-cba2-4545-a0de-7fe4c85a1078)
![image](https://github.com/user-attachments/assets/8aa60a5a-abec-4ff1-8db4-7f3f68ef3f59)
![image](https://github.com/user-attachments/assets/47dac09c-c109-48f4-8e2d-bc3dcc7e9659)
![image](https://github.com/user-attachments/assets/7634aee3-339f-4969-979b-2fd79411a685)
![image](https://github.com/user-attachments/assets/13fc8197-577d-4882-825b-7292db7f709b)

# Data Warehousing
![image](https://github.com/user-attachments/assets/74940346-87b2-4544-b36a-f4abeadc55a6)

![image](https://github.com/user-attachments/assets/dd0707ae-d322-4634-b255-13103f638617) ![image](https://github.com/user-attachments/assets/8f555754-2823-4221-ad5b-8aa1ebc6c671)

![image](https://github.com/user-attachments/assets/a68c108c-a87d-4b0d-a134-891a2664ba12)
![image](https://github.com/user-attachments/assets/01d25571-f36b-4b6f-9fc8-fa823b78d09e) ![image](https://github.com/user-attachments/assets/439df014-61a1-4dd6-9608-0967dada8064)

![image](https://github.com/user-attachments/assets/89dabdc2-59ba-4554-b524-d0378ea740a9) ![image](https://github.com/user-attachments/assets/a3af4d2c-fc34-47ea-b7fb-f473c8c0985e)
![image](https://github.com/user-attachments/assets/7eb4b314-3d4e-46a3-93a7-8adc51215157) ![image](https://github.com/user-attachments/assets/78e7bbbb-dd0d-4f89-adc4-bfc2f619faea)

### Analysis Services - ![image](https://github.com/user-attachments/assets/1713af2e-dd56-4ae0-a46a-d5b931b5e4e0)

![image](https://github.com/user-attachments/assets/d08abb86-ebfd-4803-acd0-7b4d4720118a) ![image](https://github.com/user-attachments/assets/4487301d-5bd1-47d7-aa5e-ed4f40383624)

![image](https://github.com/user-attachments/assets/b39f7d2a-81ba-46ad-bc82-92edec5450ef)

![image](https://github.com/user-attachments/assets/6bb2b357-9a1e-4eff-bad2-7e6aed767cc7)

* A data lake and a data warehouse both store large quantities of data. However, a data lake holds raw data while a data warehouse holds structured information.

* Data presented to the Azure Data Factory may originate from many formats including relational and non-relational formats.
* The Control node is the brain of the architecture. It's the front end that interacts with all applications. The MPP engine runs on the Control node to optimize and coordinate parallel queries. When you submit a processing request, the Control node transforms it into smaller requests that run against distinct subsets of the data in parallel.
* Use Azure Synapse Analytics for very high volumes of data (multi-terabyte to petabyte sized datasets). over Azure Analysis Service.
### Ingest data using Azure Data Factory
![image](https://github.com/user-attachments/assets/49f0d0cd-b1eb-411f-8fca-37ec4a84bda8) ![image](https://github.com/user-attachments/assets/05638b33-4560-4dd2-b6dc-fa3546f6a788)
![image](https://github.com/user-attachments/assets/ba90738c-43d0-4e3d-b35a-867f33990322) ![image](https://github.com/user-attachments/assets/4a911011-8d12-47f8-8259-48c7ac43f3af)

![image](https://github.com/user-attachments/assets/345d4d30-8acc-4b7e-b87b-1dadbc638dd8) ![image](https://github.com/user-attachments/assets/06a5633a-7415-4fe6-b274-ab55a15d6b38)

![image](https://github.com/user-attachments/assets/e8c42928-8fab-449f-973a-ac36c302929c)
### Polybase
![image](https://github.com/user-attachments/assets/220aa33b-7d6d-4da5-b5d1-8ab9742d5ec0) ![image](https://github.com/user-attachments/assets/3b3ac2ff-ee1f-4091-bb08-e89b0e41cbb5)
* Orchestration is the process of directing and controlling other services, and connecting them to allow data to flow between them. Data Factory uses orchestration to combine and automate sequences of tasks that use different services to perform complex operations.
* * Data Factory moves data from a data source to a destination. A linked service provides the information needed for Data Factory to connect to a source or destination such as Azure Blob Storage linked service to connect a storage account to Data Factory, or the Azure SQL Database linked service to connect to a SQL database.
  * You can use mapping to transform your data from the input format to the output format. The columns from the input data can be mapped to the data format required by the output.
* SSIS is an on-premises utility. However, Azure Data Factory allows you to run your existing SSIS packages as part of a pipeline in the cloud. This allows you to get started quickly without having to rewrite your existing transformation logic.
* PolyBase is a feature of SQL Server and Azure Synapse Analytics that enables you to run Transact-SQL queries that read data from external data sources.
* Azure Databricks → A big data analytics and machine learning platform based on Apache Spark, used for processing large-scale data workloads.
* Azure Data Factory → A data integration and orchestration service that helps in ETL (Extract, Transform, Load) processes.
* Azure Synapse Analytics → A data warehouse and analytics platform that enables big data processing and querying using T-SQL and Spark.
  * Azure Data Studio → Incorrect, because Azure Data Studio is a database management and query tool
  * Azure Synapse Analytics is a generalized analytics service that supports multiple technologies for processing data:
* Transact-SQL (T-SQL) → Used for querying and managing structured data in Synapse's dedicated and serverless SQL pools.
* Spark → Used for big data processing, machine learning, and analytics through Apache Spark pools in Synapse.
* Azure HDInsight is a fully managed cloud-based big data analytics service that is based on Apache Hadoop. It provides an enterprise-grade Hadoop ecosystem, supporting various open-source frameworks such as:

Apache Spark (for fast big data processing)
Apache Hive (for SQL-based querying on big data)
Apache HBase (for NoSQL workloads)
Apache Kafka (for real-time data streaming)
* Apache Storm is a real-time, distributed stream processing framework that runs on Azure HDInsight. It is designed for fault-tolerant, scalable, and low-latency processing of streaming data.
* Azure Data Lake Storage (ADLS) Gen2 provides:
✔ Hierarchical File System (HFS) → Supports directories and file organization, improving performance for big data workloads.
✔ Granular Security → Uses role-based access control (RBAC) and ACLs (Access Control Lists) for fine-grained permissions.
✔ Supports both raw (blob) and structured data → It can store raw, semi-structured, and structured data, making it ideal for big data analytics and data lakes.
* Azure Data Lake Analytics uses U-SQL as its primary language for defining and executing jobs. U-SQL is a combination of SQL and C#, allowing developers to query, transform, and process big data efficiently in Azure Data Lake Storage.
* You define a job using a language called U-SQL. This is a hybrid language that takes features from both SQL and C#, and provides declarative and procedural capabilities that you can use to process data.

* Azure Synapse Analytics consists of multiple components to handle both data warehousing and big data analytics:

** Synapse SQL pool → Supports T-SQL-based querying for structured data, including dedicated and serverless SQL pools.
** Synapse Spark pool → Provides an Apache Spark-based environment for big data processing, machine learning, and analytics.
** Synapse Pipelines → A built-in ETL/ELT orchestration service, similar to Azure Data Factory, for moving and transforming data.

* Data Lake Store provides a file system that can store near-limitless quantities of data. It uses a hierarchical organization (like the Windows and Linux file systems), and can hold massive amounts of raw data (blobs) and structured data. It is optimized for analytics workloads.

* Synapse SQL pool is a collection of servers running Transact-SQL. You write your data processing logic using Transact-SQL.
* A Synapse pipeline is a logical grouping of activities that together perform a task. The activities in a pipeline define actions to perform on your data.
* * In Azure Data Factory (ADF), pipelines can be created using both a graphical user interface (GUI) and custom code:

Graphical UI (ADF Studio) → A no-code/low-code visual designer that allows users to drag and drop activities to build pipelines.
Custom Code (JSON & SDKs) → Pipelines can be defined using JSON templates, PowerShell, .NET, Python SDKs, or Azure Resource Manager (ARM) templates for automation and deployment.
** In Azure Data Lake, a staging area is a temporary storage location where raw data is initially stored before transformation and processing. It helps with:

Data ingestion from various sources (databases, APIs, IoT, etc.)
Data validation and cleaning before moving to a structured format
Performance optimization by separating raw data from transformed data

* Azure Data Lake Storage (ADLS) Gen2 is designed for big data storage and analytics and includes the following key features:

Directories and subdirectories support → Unlike traditional blob storage, ADLS Gen2 provides a hierarchical namespace, allowing for directories and subdirectories to improve file organization and performance.

Hadoop Distributed File System (HDFS) support → ADLS Gen2 is HDFS-compatible, meaning it integrates well with big data frameworks like Apache Spark, Hadoop, and Databricks.

Support for Role-Based Access Control (RBAC) → ADLS Gen2 supports RBAC and Access Control Lists (ACLs), enabling fine-grained security and access management.
* Azure Databricks is a fast, easy-to-use, and collaborative Apache Spark-based analytics service fully integrated with Microsoft Azure. It supports:

Azure Databricks is a managed SaaS offering that provides Apache Spark clusters as a service, reducing infrastructure management overhead.
Machine learning → Yes ✅

It includes MLflow for experiment tracking and integrates with Azure Machine Learning, enabling AI and ML model development.
Streaming → Yes ✅

Supports real-time data processing using Structured Streaming with Apache Spark, making it ideal for handling streaming data from sources like Kafka, Event Hubs, and IoT devices.
Big data processing → Yes ✅

Optimized for distributed big data processing, allowing users to run large-scale ETL, data transformations, and analytics on petabyte-scale datasets.
* ****** chaeck this *** PolyBase allows T-SQL queries to access and join data from external sources (such as Azure Blob Storage, Azure Data Lake, Hadoop, and other relational databases) without moving the data.

To enable PolyBase in SQL Server, client connection software (also known as ODBC drivers or connectors) is required to establish connections with external data sources.

Key Points:
PolyBase enables querying and joining external data using T-SQL.
It requires external data source connectors (like ODBC, JDBC) for integration.
Helps in virtualizing data without the need for ETL (Extract, Transform, Load) processes.
* In Azure Data Factory (ADF), ingestion tasks are triggered using pipelines, which define the workflow for data movement and transformation.

Pipeline → ✅ Correct

A pipeline is a logical grouping of activities that ingests, processes, and moves data from source to destination.
It can be scheduled, event-triggered, or manually triggered.
* * HTAP (Hybrid Transactional Analytical Processing) is a modern approach that allows organizations to analyze operational data in real-time within its original location, without needing to move it to a separate data warehouse.

Why HTAP?
Traditional data warehouses require data to be extracted, transformed, and loaded (ETL) before analysis, which adds latency.
HTAP systems allow for real-time analytics on transactional data, reducing delays and improving decision-making.
It combines transactional (OLTP) and analytical (OLAP) processing within the same system.
* This strategy is known as hybrid transactional analytical processing (HTAP). You can perform this style of analysis over data held in repositories such as Azure Cosmos DB using Azure Synapse Link.


* Apache Hive is a data warehouse system built on top of Hadoop that provides an SQL-like query language called HiveQL. It is designed for:

Querying large datasets stored in Hadoop
Aggregating and summarizing data efficiently
Transforming data using SQL-like syntax
* Apache Kafka is a distributed event streaming platform used for real-time data ingestion and processing. It is designed to:

Ingest large volumes of streaming data from various sources in real-time.
Process and distribute messages across multiple consumers efficiently.
Provide fault-tolerant and scalable streaming capabilities.

* A data lake is a repository for large quantities of raw data. Because the data is raw and unprocessed, it's very fast to load and update, but the data hasn't been put into a structure suitable for efficient analysis. You can think of a data lake as a staging point for your ingested data before it's massaged and converted into a format suitable for performing analytics.

* Pipelines can be triggered to run activities for ingesting data.

* Power BI is built on a few key building blocks that help users analyze and present data effectively:

🔹 Visualizations – Graphical representations of data (charts, graphs, maps, etc.).
🔹 Datasets – Collections of data imported or connected to Power BI.
🔹 Reports – Multi-page collections of visualizations based on a dataset.
🔹 Dashboards – Single-page collections of visualizations from multiple reports.
🔹 Tiles – Individual visualizations pinned to a dashboard.

![image](https://github.com/user-attachments/assets/438c4be8-cc22-4ae4-a4de-04b5c864ce3e)


![image](https://github.com/user-attachments/assets/65e644de-cfb5-44cb-a408-5f17c96fa019)











