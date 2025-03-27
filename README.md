# Yvette_Anamle_Data_Analyst
# Descriptive Analysis

# Project Description
Descriptive Analysis of 3-1-1 service request of the city of vancouver.

# Project Title
Analyzing 3-1-1 Service Requests: Trends and Geographic Distribution

# Objective
The objective of this descriptive analysis is to examine the most common types of 3-1-1 service requests and their geographic distribution across different local areas. By analyzing the frequency and nature of these requests, this study aims to identify trends in public service needs, highlight problem areas, and provide insights that may contribute to improving response efficiency and resource allocation.

# Dataset Overview: 3-1-1 Service Requests

The dataset contains 500 service request records related to the city's 3-1-1 system. It includes details about the type of request, department handling it, status, timestamps, location data, and service channels.

Key Features:

Department: The city department responsible for the request.

Service Request Type: The nature of the request (e.g., vegetation maintenance, street operations).

Status: Indicates whether the request is open or closed.

Closure Reason: If closed, the reason for closure (e.g., service provided, referred to another group).

Timestamps:

Open timestamp (when the request was initiated).

Close date (if applicable).

Last modified timestamp.

Address & Local Area: Geographic details of where the request was made.

Channel: The platform through which the request was received (e.g., social media).

Latitude & Longitude: Coordinates for mapping service requests (missing in some records).

Geom: A formatted string containing latitude/longitude data.

Data source: dataset was downloaded from the city of Vancouver website 
https://opendata.vancouver.ca/pages/home/

Dataset: 3-1-1 service request: 

 https://opendata.vancouver.ca/explore/dataset/3-1-1-service-requests/table/?disjunctive.service_request_type&disjunctive.status&disjunctive.channel&disjunctive.local_area&disjunctive.department&disjunctive.closure_reason 

 # Methodology
 **Data Ingestion**

The data ingestion stage is responsible for collecting, storing, and organizing raw service request data before further processing and analysis. This step ensures that data is efficiently structured for querying and retrieval in downstream processes.

 Data Source and Initial Upload
The dataset is obtained from the City of Vancouver's website, which provides open data on 3-1-1 service requests. The raw data is downloaded in CSV format (3-1-1-service-requests.csv) and uploaded to an Amazon S3 bucket named service-request-raw-yve. This cloud-based storage solution enables scalability, durability, and easy integration with other data processing tools.

 Partitioning for Efficient Querying
To enhance retrieval efficiency, the data is organized using a time-based partitioning strategy. The dataset is structured into partitions based on:

Year

Quarter

Month

Day

This hierarchical organization helps optimize performance by allowing query engines to scan only relevant partitions, significantly improving the speed and efficiency of data retrieval.

The primary objective of this data ingestion step is to store and organize raw service request data in Amazon S3 for further processing. By implementing time-based partitions, the system ensures efficient querying and retrieval, allowing for faster access to relevant data. This structured approach facilitates future transformations, aggregations, and analytics by maintaining a well-organized raw data layer. Additionally, it supports seamless integration with analytics and machine learning workflows, ensuring smooth downstream processing. Ultimately, this ingestion framework provides a strong foundation for data analysis, enabling the identification of trends in 3-1-1 service requests and supporting data-driven decision-making.

Figure: 1.1 
 Existing file in the S3 bucket following the data ingestion process.
 
<img width="740" alt="data injegtion" src="https://github.com/user-attachments/assets/fe3622d9-4c55-428a-a4ae-b3dce064362c" />

Note:screenshot taken from my aws console

 **Data profiling**

This is a critical step in the data preparation process that helps identify inconsistencies, anomalies, and overall data quality issues. This step ensures that the dataset is clean, reliable, and suitable for analysis. It involves analyzing the structure, content, and relationships within the data to detect missing values, duplicates, and incorrect data types.

In this project, AWS Glue DataBrew is used to perform data profiling. A project named 3-1-1-req-prj-yvete is created, and the dataset is imported from the service-request-raw-yve S3 bucket. Once the dataset is uploaded, AWS Glue DataBrew automatically generates a data profile, which provides a comprehensive overview of the dataset. The profiling process captures key insights, including:

Total number of rows and columns – Helps determine dataset size.

Data types of each column – Identifies numerical, categorical, and textual data.

Missing values – Detects gaps in the dataset that may require imputation or removal.

Duplicate rows – Flags redundant data that may affect accuracy.

Value distribution – Analyzes how data points are spread across different columns.

Outliers and anomalies – Highlights unexpected variations in the dataset.

After executing the data profiling task, the results are saved in a dedicated transformation bucket named service-request-trf-yve. This structured approach to data profiling ensures that only high-quality, well-structured data is used in subsequent processing and analysis.

Figure: 1.2  Data profile overview 

<img width="735" alt="Data profile overview " src="https://github.com/user-attachments/assets/c4117240-d133-44f0-a0c1-14484b1bbf2e" />

Note:screenshot taken from my aws console



**Data Cleaning**

Data cleaning is an essential step in the data analysis process that enhances the quality, consistency, and reliability of a dataset. Clean data ensures accurate insights and reduces errors in downstream processing and analysis. Given the nature of the dataset, specific cleaning operations are performed to standardize and improve the data. The entire cleaning process is documented and stored in the service-request-trf-yve transformation bucket.

Figure 1.3 below presents a successful data cleaning job, demonstrating the structured transformation applied to the dataset.

Key Data Cleaning Steps

**Column Renaming**
   
To maintain consistency and improve readability, column names are reformatted to follow a standardized naming convention. The following changes are applied:

Service request type is renamed to Service_request_type.

Closure reason is renamed to Closure_reason.

Service request open time stamp is renamed to Service_request_open_timestamp.

Service request close date is renamed to Service_request_close_date.

Last modified timestamp is renamed to Last_modified_timestamp.

Local area is renamed to Local_area.


**Handling Missing Values**
   
To ensure completeness and prevent inconsistencies, missing values are handled using different imputation techniques:

Service_request_close_date: Filled with the most frequently occurring value in the column.

Address: Missing values replaced with "3039 Horley Street" as a default address.

Local_area: Missing values replaced with "N/A" to indicate unrecorded areas.

Latitude & Longitude: Missing values replaced with their respective average values to maintain geographical consistency.



Figure: 1.3 Successful cleaning job 

<img width="749" alt="Cleaning recipe " src="https://github.com/user-attachments/assets/4d5a41b2-5780-435b-b6e6-16e60fef2eb6" />

Note:screenshot taken from my aws console



Figure 1.4 provides a detailed view of a successful job run for the data cleaning process, confirming that all cleaning steps were executed without errors. This includes column renaming, handling missing values, and standardizing data formats to improve overall data quality. The successful completion of the job run ensures that the dataset is well-structured, accurate, and ready for further analysis



Figure: 1.4 Successful cleaning job 

<img width="730" alt="Successful cleaning job  " src="https://github.com/user-attachments/assets/eb36e1df-8eed-4c88-ae3a-5ed8ca78f7d3" />

Note:screenshot taken from my aws console


**Data Cataloging**

The AWS Glue Data Catalog serves as a centralized metadata repository that organizes, tracks, and manages data across various AWS services. It acts as a smart organizer, making it easier to discover, query, and maintain datasets while ensuring security and compliance. By cataloging data effectively, organizations can enhance data governance and streamline analytics workflows.

To perform data cataloging, a crawler is created in AWS Glue. This crawler is responsible for scanning data sources, automatically detecting schemas, and populating the AWS Glue Data Catalog with structured metadata tables. This process simplifies data discovery and querying, allowing for seamless integration with analytical services.

The displayed screenshot showcases a crawler named service-request-crw-yve, which successfully ran and created a new table in the catalog. This newly generated metadata table allows services like Amazon Athena and AWS Glue ETL jobs to efficiently interact with structured data. These services will be particularly useful in subsequent steps, such as data summarization and transformation.

Additionally, the crawler’s log provides valuable insights into the scanning process. It captures details such as schema changes, detected anomalies, and any potential processing issues, helping maintain the integrity and accuracy of the cataloged data. Through this automated metadata management approach, data remains well-organized, up to date, and ready for analysis.

Figure: 1.5 Successful crawler run 

<img width="614" alt="Successful crawler run" src="https://github.com/user-attachments/assets/b55f11a2-aa00-4fb0-ae4e-973eb6e1d2e9" />

Note:screenshot taken from my aws console


The AWS Glue Data Catalog interface displays the creation of a database named "service-request-data-catalog-yve", which serves as a structured repository for metadata. Within this database, a table named "sev_req_trf_systems" has been generated by running the crawler. This table is stored in an S3 bucket (service-request-trf-yve) in Parquet format, ensuring optimized storage and efficient data retrieval.

Storing the table in Parquet format enhances query performance and reduces storage costs due to its columnar data structure, which is particularly beneficial for analytical workloads. With this setup, services such as Amazon Athena, AWS Glue ETL jobs, and other data processing tools can efficiently query, analyze, and transform the data, supporting seamless data integration and analysis workflows.


Figure: 1.6 Catalog table

<img width="615" alt="Catalog table " src="https://github.com/user-attachments/assets/9d33e47e-740c-437d-95dc-c08291d29800" />

Note:screenshot taken from my aws console


**Data summarization**

Data summarization is performed using an ETL (Extract, Transform, Load) job that processes raw data, applies various transformations, and loads the structured data into designated storage for further use. This process not only enhances data quality but also provides a visual flow that facilitates analytics and reporting.

1. Data Extraction

The ETL job retrieves data from the AWS Glue Data Catalog, ensuring that the latest and most relevant datasets are accessed. This step is crucial for maintaining consistency across multiple data sources.

2. Data Transformation

Several transformation steps are applied to clean, structure, and summarize the data before it is stored. These steps include:

a. Schema Adjustments

Drop Unnecessary Columns: To maintain compatibility and efficiency, the following columns are removed:

Latitude

Longitude

Geo

b. Filtering Records

Data is filtered based on the status field, ensuring that only relevant records that match the specified criteria are retained. For example, records from specific locations such as:

Local Area

Downtown

c. Date Transformation

The license issued date is transformed to only retain the year for better summarization and trend analysis.

A new field, Report_Date, is added to the summary to provide a timestamp for tracking processed records.

Report_Date is then converted to Report_Date_LTZ to align with the required time zone formatting.

d. Aggregation and Grouping

Data is grouped by the "department" field to create structured summaries.

The aggregation function computes the average ("avg") of the "service_request_open_timestamp" field, helping to analyze service request response times.

e. Final Schema Preparation

After the transformations, the Report_Date column is removed to finalize the schema before loading.

3. Data Loading

Once the transformed data is ready, it is loaded into different destinations for structured storage and further usage:

System Logs Target:

Data is converted into a single structured file format for system logging purposes.

This structured log enables better monitoring and debugging.

User Data Target:

The final transformed dataset is loaded into Amazon S3 (Sev-req-cur-yve) for user access.

This dataset is structured for ease of access, reporting, and visualization.

Figure: 1.7  Service request visual ETL

<img width="611" alt="Service request summarization " src="https://github.com/user-attachments/assets/e2e7bc7b-55de-418e-9c41-98f07d1579f3" />

Note:screenshot taken from my aws console


The output schema displayed in the AWS Glue Visual ETL job represents the structure of the finalized, summarized dataset prepared for loading. It contains three key fields: department (string), avg_service_request_outcome (double), and Report_Date_LTZ (timestamp). The department field identifies the originating department for the summarized records, which in this case may remain constant but allows for future scalability across multiple departments. The avg_service_request_outcome field captures the computed average of a key performance metric—likely derived from aggregating numerical values such as costs or counts during transformation. This field uses the double data type to preserve precision in the calculation. Finally, Report_Date_LTZ is a timestamp that automatically records when the summarization job was executed, providing traceability and enabling time-based filtering or auditing. This structured schema ensures the dataset is optimized for querying, reporting, and integration with downstream analytics tools such as Amazon Athena or QuickSight.


Figure: 1.8 ETL jobs output schema 

<img width="622" alt="ETL jobs output schema " src="https://github.com/user-attachments/assets/fab7b1cb-e850-4aa3-bc27-d5e3590ba4ef" />

Note:screenshot taken from my aws console



This successful ETL pipeline job run in AWS confirms that data has been extracted, transformed, and loaded correctly into the target system. The output schema defines the structured format of processed data, including column names, data types (e.g., string, double, timestamp), and ensures data quality. AWS provides a data preview, job run metadata, and validation checks to confirm accuracy. The processed data can then be integrated with services like Amazon S3, Redshift, or RDS for further analysis, reporting, or machine learning applications.



Figure: 1.9 

Pipeline job run output 

<img width="623" alt="Pipeline job run output " src="https://github.com/user-attachments/assets/ff0e3129-1405-4391-b498-8a914002bea3" />

Note:screenshot taken from my aws console


The image displays in figure 1.10 contains the contents of an Amazon S3 bucket under the path:sev-req-cur-yve/3-1-1-service-request/metrics/systems/Report_Date_LTZ=2025-03-02 21:45:56.8412/.

This folder contains summarized data organized by department under various engineering-related systems. Specifically, it includes 13 departmental folders, each representing a subset of metrics or reports generated for that timestamp. The departments include:

ENG – Film and Special Events Office

ENG – Integrated Graffiti Management

ENG – Sanitation Services

ENG – Sewer Operations

ENG – Solid Waste Programs

ENG – Street Use Management

ENG – Streets Design Branch

ENG – Streets Furniture

ENG – Streets Operations

ENG – Traffic and Data Management

ENG – Traffic and Electrical Operations and Design

ENG – Waterworks Design

ENG – Waterworks Operations

Figure 1.10 Summarised data for systems under reported date folder 

<img width="623" alt="ummarised data " src="https://github.com/user-attachments/assets/fde24dab-d90a-4a4c-8242-95fa3f30e7cb" />

Note:screenshot taken from my aws console

This file, located in the Amazon S3 bucket under the User directory for the specified report date, is the result of a user-specific data processing run performed by AWS Glue as part of the  ETL (Extract, Transform, Load) workflow above. It represents the final output of a Glue job that processed raw service request data related specifically to users, transforming it into a summarized format suitable for reporting, analytics, or downstream applications.

Figure: 1.11 Summarized data for user under reported date folder 

<img width="631" alt="user" src="https://github.com/user-attachments/assets/35557e19-7c8a-4130-b74d-3887682e25f5" />

Note:screenshot taken from my aws console



The image below displays the AWS Glue Console's Data Catalog Tables section. It shows two tables—sev_req_metric and sev_req_trf_systems—both stored in the service-request-data-catalog database with data located in Amazon S3 and classified in Parquet format. 


Figure: 4.13  New metrics table for summarized result 

<img width="626" alt="New metrics table" src="https://github.com/user-attachments/assets/f7b7e7ea-0689-4063-95cb-d53f4d725aaf" />

Note:screenshot taken from my aws console





























