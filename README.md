# The Azure DataBricks Medallion Project

Azure databricks medallion architeure is a comprehensive data management system that includes a bronze layer,silver and gold layers. It involves raw storage, real-time streaming, metadata governance, and pipeline validation. 

** Tools and Technologies: **
The project was built using *Azure Data Factory, Azure Storage, Databricks, Unity Catalogue, Kafka, and Git. Also the use of SQL and Python programing, PowerBi was also used for analytics *.
###  BRONZE

The bronze layer includes Azure Storage, Databricks, and Autoloader for stream ingestion. 
 ### SILVER
The silver layer converts unstructured data into query-ready tables that replicate a a retail's finance platform's business logic.
The Silver layer converts basic data feeds and system events into structured, enhanced, and deduplicated streams for further processing in Gold. 
*The Silver layer uses* * Azure Storage, Databricks (Streaming + Delta Lake), SQL, PySpark, Unity Catalogue, and Python*
Each table utilizes the IDEMPOTENT UPSERT LOGIC, which allows for efficient aggregations and deduplication during event time periods. The Silver layer also provides time intelligence with user bins, classifying users into ranges.

The Silver was tested, which include watermarking, deduplication, priority idempotency, lightweight enrichment, state control in joins, and state control in joins. 
The project offers a tidy workstation for data professionals working with mainframe-based ETL or cloud-based pipelines. Enhancing productivity and boosting organisational efficiency.

## Design of the Azure DataBricks Medallion Project
 ### **Technologies and tools**
*SQL Python & PySpark; -Cloud Source Storage System (Azure Storage); -Databricks and Unity Catalogue; -Kafka (CDC + Sensors), git Databricks (Streaming + Delta Lake) 
### Bronze 
A functional bronze layer was created that included raw storage, real-time streaming, metadata governance, and pipeline validation. Supporting ETL accross fields and expertise for effective decision intelligence.
** The Bronze Layer of a Medallion Architecture was constructed by me using: **
#### Provide External Storage 
* The first raw feed was created by ingesting company data from on-premises to ADLS using Azure Data Factory. supporting enhanced cloud to on premise connectivity. *
* External location was configured for data access, landing zone folder and  and checkpoint zones were created, connects to the data in azure in an automated process.
#### Turn the Bronze Tables & Catalogue Around 
Workspace and azure data storage were linked, following the creation of an ADLS
The "Bronze-layer" Delta tables were then registered for raw ingestion. 
The untouched source is reflected in these raw tables, which is a necessary step prior to any cleaning or change. 
#### Integrating Time Intelligence From the Start 

Timestamp was used for date mapping to join streaming Kafka data later. 
#### Using Autoloader for Stream Ingestion 

With Databricks Autoloader leveraging CloudFiles, the true magic began. Using schema enforcement, checkpoints, and add mode logic, I streamed structured CSV data into Bronze tables.
Last step: Validations 
Connections tests were implemented  to verify each table's creation and registration before finishing. To avoid automation failures, this verifies that each table is registered and ready for downstream processing. This prevents pushing downstream logic into a faulty setup, particularly in automated pipelines like CI/CD. 

### SILVER 
SILLVER LAYER, converts unstructured data into query-ready tables that replicate a retail business logic. The basic data feeds and system events are converted into structured, enhanced, and deduplicated streams at this layer so they may be further processed in Gold. 

#### Silver Table Creation
Outlining each table's composition and function: Through timestamp comparisons, primary key matching, or MERGE conditions with structured streaming, each table makes advantage of the IDEMPOTENT UPSERT LOGIC.

#### Construct Dependable Merge Streams
I handled inserts for new user records using streaming merge logic, which Spark uses. DropDuplicates() was used to eliminate exact duplicate records, and watermarking was used in streaming to control late data so that aggregations and deduplication could be done efficiently during event time periods. to retrieve updated user information, such as name and date of birth, from Kafka. For timestamps, I use a rank() window function to ensure that just the most recent change for each user is applied.
#### Provide Intelligence with User Bins: 
To classify users into ranges
