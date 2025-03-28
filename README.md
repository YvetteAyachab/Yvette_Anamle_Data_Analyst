# Yvette_Anamle_Data_Analyst

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

 # 1-	Data Collection and Preparation

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


Figure: 1.12  New metrics table for summarized result 

<img width="626" alt="New metrics table" src="https://github.com/user-attachments/assets/f7b7e7ea-0689-4063-95cb-d53f4d725aaf" />

Note:screenshot taken from my aws console


# 2-	Descriptive Statistics

The 3-1-1 service operated by the City of Vancouver is a vital non-emergency communication channel that enables residents to report a wide range of local issues such as potholes, graffiti, noise complaints, streetlight outages, and illegal garbage dumping. This system is designed to streamline the handling of non-urgent municipal concerns while ensuring that the 9-1-1 emergency line remains available for critical and life-threatening situations. By providing a direct line to civic services, 3-1-1 contributes significantly to the upkeep of public infrastructure and the overall quality of life in Vancouver’s neighborhoods.

As urban populations grow and cities face increasing demands on public resources, the data generated from 3-1-1 service requests has become an important source of information for municipal planning and decision-making. Each service request represents a real-time interaction between residents and city services, offering insights into the issues that matter most to the public. By analyzing this data through descriptive statistical methods, we can uncover patterns, assess service delivery performance, and detect recurring problems that may otherwise go unnoticed.

The primary objective of this proposal is to conduct a descriptive statistical analysis of 3-1-1 service requests in Vancouver, focusing on three key areas: the rate of requests over time, the nature and categorization of the issues reported, and their geographic distribution across the city. This analysis seeks to identify trends in public service demands, highlight neighborhoods or districts with higher concentrations of reported issues, and provide evidence-based recommendations that can support better prioritization, faster response times, and more effective use of city resources.


Descriptive Question 1: Top 10 most common service requests 

This query helps identify the most common issues people report, so the city can manage resources better and improve services. The result of this query showed that the most common issue was vegetation encroachment of city property cases followed by abandoned non-recyclable small cases' identifying these will help us improve response and find ways to reduce problems. 


Figure 2.1  SQL Query and result for question 1 using Athena 

<img width="620" alt="SQL Query  1" src="https://github.com/user-attachments/assets/99b6f660-e7df-4c05-9082-7515a5994735" />

Note:screenshot taken from my aws console

The SQL query groups data by the service_request_type column from the sev_req_trf_systems table and counts the total number of requests for each type, ordering them in descending order and limiting the result to the top 10 request types. The results table shows that the most frequently reported issue is "Vegetation Encroachment of City Property Case" with 52 requests, followed by "Abandoned Non-Recyclables-Small Case" (33 requests) and "Street Cleaning and Debris Pick Up Case" (30 requests). This analysis provides a quick overview of the most common non-emergency issues reported by residents, helping the city prioritize maintenance and resource allocation more effectively. 



 
Descriptive Question 2: Service request distribution by local area. 

This query identifies how service requests are distributed by local areas and identifying the high problem areas so the city can manage resources better and improve services. This showed downtown had the highest with call type Abandoned Non-Recyclables-Small Case. 


Figure 2.2 SQL Query and result for question 2 using Athena 

<img width="624" alt="SQL Query 2" src="https://github.com/user-attachments/assets/9ef87fd9-9965-4364-b037-67f3f11f700a" />

Note:screenshot taken from my aws console


The SQL query groups the data by both local_area and service_request_type, and then counts the number of requests for each combination. The results are ordered alphabetically by local 
area and then by the frequency of service requests in descending order. The visible portion of the results shows that in the Arbutus Ridge neighborhood, multiple types of service requests were logged, including "Water Conservation Violation Case" (2 requests), as well as other issues like "Sanitation Operations Inquiry" and "Abandoned Non-Recyclables" with 1 request each. 
This analysis is useful for identifying which types of non-emergency issues are reported in specific areas, allowing city officials to localize maintenance efforts and tailor responses based on neighborhood-specific concerns.


**Data Security**

AWS Key Management Service (KMS) plays a critical role in securing data stored in Amazon S3 by providing robust encryption for data at rest. In this architecture, KMS is used to encrypt all objects stored in the S3 buckets, including the raw, transformed (trf), and curated (cur) data layers, as illustrated in Figures 2.4, 2.6, and 2.8 respectively. This ensures that all data, regardless of its processing stage, is protected from unauthorized access and meets stringent security and compliance requirements.

In addition to encryption, versioning is enabled on each S3 bucket. This feature maintains historical versions of every object stored, which is particularly useful for tracking changes, recovering from accidental deletions or overwrites, and supporting auditability. By preserving older versions of data, the system provides an added layer of data integrity and operational resilience.

To further enhance data durability and disaster recovery, S3 replication rules are configured to automatically replicate data from the primary buckets to designated backup buckets, as depicted in Figures 2.5, 2.7, and 2.9. This cross-region or same-region replication ensures that a secondary copy of the data is always available, supporting business continuity, compliance with data retention policies, and faster recovery in the event of service disruptions or data loss in the primary environment.


Figure 2.3 Creating the Key in KMS 

<img width="630" alt=" Key in KMS " src="https://github.com/user-attachments/assets/9fafe3f1-fdda-4cf3-8a3f-f199ce75a5b5" />

Note:screenshot taken from my aws console


 A new KMS key has been successfully created, as indicated by the green success banner at the top. The key is named using the alias 3-1-1-req-key-yve, and it has been assigned a unique key ID.
The key is currently enabled, which means it is active and available for cryptographic operations. It is a symmetric key, meaning the same key is used for both encryption and decryption processes, and it follows the SYMMETRIC_DEFAULT specification. The designated usage for this key is "Encrypt and decrypt", which is typical for securing data at rest in Amazon S3.

This key will be used to encrypt 3-1-1 service request data stored in different S3 buckets (raw, transformed, and curated), ensuring that the data remains protected against unauthorized access while meeting compliance and security standards.


Figure 2.4 Raw Bucket: Data Encryption the S3 

<img width="619" alt="Data Encryption the S3" src="https://github.com/user-attachments/assets/955a9f03-6f02-4a7f-8a6a-aaaffe605f27" />

Note:screenshot taken from my aws console


Figure 2.5 Raw bucket: Data Replication for the S3 bucket

<img width="615" alt="Screenshot 2025-03-26 at 8 07 21 PM" src="https://github.com/user-attachments/assets/7770353a-8883-4cec-bf72-68f2bb869511" />

Note:screenshot taken from my aws console


Figure 2.6 Trf Bucket: Data Encryption for S3 bucket 

<img width="625" alt="Screenshot 2025-03-26 at 8 08 58 PM" src="https://github.com/user-attachments/assets/e34a7e62-d63f-4bb4-b9ca-715277ffc84f" />

Note:screenshot taken from my aws console


Figure 2.7  Trf Bucket: Data Replication for the S3 bucket

<img width="617" alt="Screenshot 2025-03-26 at 8 17 14 PM" src="https://github.com/user-attachments/assets/b229d4d8-a3d9-4caf-9c2c-319e7e098c06" />

Note:screenshot taken from my aws console


Figure 2.8 Cur Bucket: Data Encryption for S3 bucket 

<img width="599" alt="Screenshot 2025-03-26 at 8 19 58 PM" src="https://github.com/user-attachments/assets/c65bb6c7-127a-4bf3-83d2-a72a885b0491" />

Note:screenshot taken from my aws console



Figure 2.9 Cur Bucket: Data Replication for the S3 bucket

<img width="618" alt="Screenshot 2025-03-26 at 8 20 44 PM" src="https://github.com/user-attachments/assets/ca47d9e9-69ab-49bb-808c-5ddb1c13583a" />

Note:screenshot taken from my aws console


**Data Governance**

 ETL Pipeline
 
To ensure the integrity, reliability, and usability of 3-1-1 service request data, a robust ETL (Extract, Transform, Load) pipeline has been implemented using AWS Glue. This pipeline plays a key role in automating the data preparation process, while also embedding critical data governance principles—particularly around data quality. As shown in Figure 2.10, the ETL job extracts raw data from the S3 raw bucket, applies a series of transformation and validation rules, and then loads the results into categorized output folders within the S3 transformed (trf) bucket.

A major focus of this ETL process is on data quality checks, which are divided into three core dimensions: completeness, uniqueness, and data freshness. These checks are vital to ensure the data meets established thresholds before it is curated for reporting or analytics purposes.

Completeness

Completeness refers to the proportion of non-null, valid values present in key fields. The ETL job checks for:

Address field: must be ≥ 80% complete
Local area field: must be ≥ 95% complete
Service request close date: must be ≥ 97% complete
These thresholds ensure that essential data elements are sufficiently populated to allow for meaningful analysis.
Uniqueness

Uniqueness checks evaluate the level of data duplication across key fields, which is critical for avoiding redundant records and ensuring analytical accuracy. The applied rules include:

Local area field: must exhibit a uniqueness ratio of greater than 0.04
Address field: must be ≥ 70% unique

Data Freshness

Freshness checks determine how up-to-date the data is, based on the recency of the service request close date. For this dataset, the acceptable threshold is:

Service request close date: must be ≥ 50% fresh (i.e., not stale or outdated)
Quality Check Output Handling

Following the execution of these data quality checks, the ETL job segregates records into two distinct folders:

Passed folder: Contains records that meet all specified quality thresholds and are ready for further transformation or analysis.

Failed folder: Stores records that fall short of one or more criteria and may require manual review or reprocessing.

As shown in Figures 2.12 and 2.13, this separation allows for better traceability, error resolution, and data re-validation workflows. It also supports a continuous improvement approach to data governance, where problematic patterns can be identified and addressed over time.

By embedding these data governance rules directly into the ETL pipeline, the solution promotes data trustworthiness, supports operational reporting, and ensures that downstream systems and stakeholders are working with high-quality, reliable data.


Figure 2.10 Data brew pipeline for data quality control

<img width="620" alt="Screenshot 2025-03-26 at 8 39 33 PM" src="https://github.com/user-attachments/assets/b3f5a21d-0e7e-4a16-be6b-5ccc8927ad07" />

Note:screenshot taken from my aws console


Figure 2.11 Output Schema for the ETL result 

<img width="621" alt="Screenshot 2025-03-26 at 8 42 44 PM" src="https://github.com/user-attachments/assets/b1c1a8a3-e600-41bc-a20a-0eb6f10c989c" />

Note:screenshot taken from my aws console


Figure 2.12  Failed Folder in S3 Bucket 

<img width="616" alt="Screenshot 2025-03-26 at 8 45 00 PM" src="https://github.com/user-attachments/assets/c39582be-8212-406a-a8d5-f742e7b2ab1b" />

Note:screenshot taken from my aws console


Figure 2.13 Passed Folder in the S3 Bucket 

<img width="626" alt="Screenshot 2025-03-26 at 8 46 17 PM" src="https://github.com/user-attachments/assets/ad2ea1bb-6856-4e13-ab9b-1c5bf1d9125b" />

Note:screenshot taken from my aws console



**Data Monitoring**

Effective data monitoring is essential for maintaining system performance, managing storage costs, and ensuring the reliability of data pipelines. In this architecture, Amazon CloudWatch is used as the central monitoring tool to track the usage of Amazon S3 storage across three key buckets: raw, transformed (trf), and curated (cur). These buckets represent different stages of the data lifecycle—from ingestion to transformation to final curated datasets ready for analysis.

CloudWatch continuously collects and analyzes storage metrics for each of these buckets, providing real-time visibility into how much data is being stored over time. This allows data engineers and administrators to proactively monitor usage trends, identify unexpected spikes, and ensure that storage capacity remains within defined operational thresholds.

CloudWatch Metrics

CloudWatch Metrics are configured to capture the size of each bucket at regular intervals. These metrics help track storage growth patterns, which are essential for capacity planning, cost optimization, and identifying inefficiencies in data retention. By analyzing historical trends over the monitoring period, stakeholders can make informed decisions about data archival, deletion, or optimization.

CloudWatch Alarms

To prevent potential overuse of resources, CloudWatch Alarms have been configured for each bucket. These alarms are triggered when the bucket size exceeds predefined storage thresholds, ensuring timely notifications and allowing for immediate corrective actions. Notifications can be sent via Amazon SNS (Simple Notification Service) or integrated with automated remediation processes.

Monitoring Configuration

For the purposes of this report, the monitoring period spans 3 months, with the following storage limits assigned to each S3 bucket:

Raw bucket: 200,000 units

Transformed (trf) bucket: 500,000 units

Curated (cur) bucket: 100,000 units

These limits are set based on expected data volumes and operational requirements. Exceeding these thresholds may indicate inefficiencies in the ETL pipeline, delays in archival processes, or unexpected increases in incoming data volume. With CloudWatch in place, such events can be detected early and resolved quickly to maintain system stability and avoid unnecessary storage costs.

Figure 2.14 Cloud Watch Dashboard 

<img width="623" alt="Screenshot 2025-03-26 at 8 58 15 PM" src="https://github.com/user-attachments/assets/8abeeedf-6318-4ec3-bf68-e0ee2b613e83" />

Note:screenshot taken from my aws console


AWS CloudTrail is like a security camera for your Amazon Web Services (AWS) account. It keeps track of everything people do—like logging in, changing settings, or using services. This is super helpful if you want to see who did what, when they did it, and from where.

It’s mainly used to check for mistakes, find out if someone did something they weren’t supposed to, or to just keep a record of everything for safety and rules (aka compliance). Once it's set up, CloudTrail automatically creates logs—kind of like a digital diary of all the activity happening in your account.


Figure 2.15 Cloud Trail for S3 Bucket 

<img width="622" alt="Screenshot 2025-03-26 at 9 01 05 PM" src="https://github.com/user-attachments/assets/700b42de-9266-4c68-9e8b-88e03fa89d8c" />

Note:screenshot taken from my aws console


# 3-	Data Visualization

Figure 3.1 draw.io architectural diagram

<img width="1258" alt="Screenshot 2025-03-27 at 1 04 04 PM" src="https://github.com/user-attachments/assets/67dc13eb-3e64-495f-ae23-26d2967ca23d" />

Note:screenshot taken from my draw.io daigram


This architectural diagram represents a cloud-based data processing and analytics system designed to collect, process, transform, catalog, and analyze 3-1-1 municipal service request data. Built primarily using AWS services, the system ensures scalability, security, automation, and redundancy across all layers of the data lifecycle—from ingestion to actionable insights.

**Data Ingestion and Raw Data Storage**

At the entry point of the architecture, data is sourced from various service request servers through defined endpoints like:

/service-request/3-1-1-request/year=2025/quarter=01/month=01/day=20/server=SRGYS-Yve
/service-request/3-1-1-request/year=2025/quarter=01/month=01/day=05/server=SRWA-Yve
These endpoints suggest a partitioned file structure, allowing data to be organized by year, quarter, month, day, and server, which facilitates efficient querying and filtering during downstream analytics.

The ingestion process is performed by the Data Team or users with specific access roles such as Role D, who interact with the system through the internet to either push raw data into the platform or configure automated pipelines.

The raw data is initially collected and stored in Amazon S3, under the bucket labeled service-request-raw-yve. This acts as the raw zone of the data lake, holding unprocessed data in its original form.

**ETL and Data Transformation Process**

Once data lands in the raw zone, it undergoes ETL (Extract, Transform, Load) operations through a combination of:

AWS Glue Jobs: These automate data extraction from S3, apply transformations (e.g., cleaning, filtering, restructuring), and prepare data for downstream use.

AWS Glue DataBrew: A visual data preparation tool used for profiling, cleansing, normalizing, and enriching data without writing code. It's particularly useful for business users and data analysts.

Key Management Service (KMS): Integrated into the pipeline to ensure secure encryption and key management, especially when handling sensitive or regulated data.

After transformation, the cleaned and structured data is stored in the transformation layer, in a separate S3 bucket labeled service-request-trf-yve.

**Data Summarization and Curation**

Transformed data is further processed and summarized using designated ETL jobs for metric aggregation, performance analysis, and trend reporting. This phase includes:

service-req-Summarization: This module consolidates data by computing aggregate values (e.g., counts per service type, department, or neighborhood).

sev-req-cur-yve: Represents the curated zone where validated, summarized, and analysis-ready datasets are stored. This layer supports operational reporting and business intelligence.
Additional structured data is stored under:

service-request/3-1-1-request/metrics/ – A dedicated path for storing service metrics such as average closure times or frequency of request types.


**Structured Data Catalog and Schema Management**

To enable fast and efficient querying of structured data, the system utilizes the AWS Glue Data Catalog. Under the catalog service-request-data-catalog-yve, two main tables are registered:

3-1-1-request: The core table storing detailed records of service requests, likely including fields like request type, timestamp, location, status, and department.

serv_req_metric: A metrics table that stores aggregated values such as service volume trends, average resolution time, and peak request periods.

The use of a catalog enables schema versioning, column-level metadata, and partition indexing, allowing for seamless integration with query engines like Amazon Athena 

**Redundancy, Replication, and Disaster Recovery** 

To ensure data durability, availability, and disaster recovery, the platform includes three replicated S3 zones:

Replication A

Replication B

Replication C

These serve as geo-redundant backups of the raw, transformed, and curated data. They also help with load balancing, failover support, and business continuity in the event of system failures or outages.

**System Integration and Access Control**

The architecture includes a well-defined role-based access control model, where team members (such as those in the Data Team) and external entities (like Role D) interact with the platform over the internet through secure endpoints.

Access is managed using:

AWS IAM roles and policies

KMS encryption keys

Network configurations that protect data in transit and at rest.




Figure 3.2 Departmental Breakdown of 3-1-1 Service Request Types

<img width="995" alt="Screenshot 2025-03-27 at 2 23 30 PM" src="https://github.com/user-attachments/assets/7cd19d28-7fd1-44a9-9492-962b17d1809c" />

Note:screenshot taken from city of vancouver site.


 **High Volume of Abandoned Item Cases Managed by ENG – Sanitation Services**
 
The chart shows a notable peak for the ENG – Sanitation Services department, especially for the Abandoned Mattress Case, with 8,196 reports.
This indicates that abandoned item cleanup is a major operational burden and likely a recurring issue in public spaces.

It May also suggest a Lack of public awareness or enforcement on proper disposal, Seasonal spikes (e.g., move-outs, evictions) and a need for targeted outreach or infrastructure (e.g., public drop-off programs)

**ENG Departments Dominate Service Volumes**

Multiple ENG (Engineering-related) departments collectively handle the majority of service request types across the city.
These include sanitation, road maintenance, street signs, trees, and infrastructure.
Indicates that engineering and operations services are the backbone of 3-1-1 response, and these teams require high resourcing and operational efficiency.

**Variety and Diversity in Request Types**

The service request types are highly diverse, ranging from:
COVID-19 feedback cases

Animal concerns

Noise complaints

Accessibility and election-related issues

Website or general feedback

Shows that 3-1-1 serves as a multi-purpose civic engagement channel, not just for street-level issues.


**Some Departments Handle Niche or Low-Volume Requests**

Departments like ACC (Accessibility) or COV Elections have lower but more focused volumes, e.g., "Election Sign Concern" or "Accessibility Feedback Case".
These low-frequency cases are critical for inclusivity and democracy, despite not having high volume.
Insight: Ensure niche departments are not overlooked in performance monitoring or platform support.


 **Potential for Duplicate or Overlapping Responsibilities**
 
Some service types (e.g., Feedback Cases, Animal Concerns) appear under multiple departments, which Suggests:

Overlap in responsibility or reporting

Possible inefficiencies or double-handling


**Opportunity to Identify Service Gaps**

If some important service types (e.g., homelessness, bylaw enforcement) show low counts, it may not always reflect fewer incidents.It might indicate:

Underreporting

Lack of public awareness

Barriers to access (language, digital literacy)


**Visual Patterns Suggest Peak Departments for Optimization**

The tallest bars (high-volume departments) may need:

Workflow optimization

Additional staff or automation

Process redesign

These peak points help prioritize which departments to support first in digital transformation, resource planning, or SLA improvement efforts.


Figure 3.4 Distribution of 3-1-1 Service Request Types by Local Area in Vancouver


<img width="1025" alt="Screenshot 2025-03-27 at 4 05 56 PM" src="https://github.com/user-attachments/assets/3d53c4aa-dcd4-454b-b6a7-0361cbce306f" />

Note:screenshot taken from city of vancouver site.


**Downtown Has the Highest Volume of Service Requests**

The data reveals that Downtown Vancouver records the highest number of 3-1-1 service requests among all local areas, with a particularly high count of 5,550 cases related to “Abandoned Non-Recyclables – Small Case.” This suggests that Downtown is a hotspot for public service demand, likely due to its dense population, high commercial activity, and significant foot traffic. The elevated number of abandoned item cases points to challenges with waste disposal in high-density urban settings. This insight highlights the need for increased resource allocation in the Downtown area—such as more frequent cleanup schedules, public education on disposal guidelines, and better enforcement of waste management bylaws.


**Abandoned Non-Recyclables – Small Case” Is a City-Wide Issue**

While Downtown reports the highest number, the “Abandoned Non-Recyclables – Small Case” request type appears consistently across nearly all neighborhoods. This pattern indicates that improper disposal of small non-recyclable items—such as discarded electronics, broken furniture, and household debris—is a widespread issue throughout the city. Its consistent presence suggests not just localized behavior, but a broader citywide challenge in public waste handling. The city may need to consider more accessible disposal services, expanded public drop-off points, and ongoing communication campaigns to encourage responsible waste behavior among residents.


**Local Variation Reflects Differences in Community Engagement or Issues**

There is notable variation in service request volumes across neighborhoods. Areas such as Renfrew-Collingwood, West End, Sunset, and Mount Pleasant show moderate to high engagement across multiple service categories. In contrast, communities like Arbutus Ridge, South Cambie, and Shaughnessy report significantly fewer requests. While lower counts may suggest fewer public issues, they could also indicate underreporting due to factors such as limited awareness of the 3-1-1 system, digital access barriers, or cultural hesitancy to report. This insight highlights the importance of equity in civic engagement and the need to assess whether all communities are equally informed and able to access city services.


** Service Requests Reflect Diverse Community Needs**

The chart displays a broad spectrum of service request types, ranging from sanitation and waste disposal to animal services, bylaw enforcement, tree maintenance, and construction concerns. This variety emphasizes that 3-1-1 services are used not just for cleanliness or infrastructure, but also for concerns related to urban planning, environmental sustainability, and even wildlife management. Departments must therefore be equipped with multidisciplinary teams and coordinated workflows to respond efficiently to this diversity of community concerns. It also indicates that public expectations from 3-1-1 systems are multifaceted and evolving.


**Community-Specific Issues Can Be Identified and Addressed Proactively**

This dataset structure allows for deeper analysis into neighborhood-specific patterns. For instance, certain service types may appear more frequently in particular areas, such as animal control cases near parks, or construction noise complaints in redevelopment zones. By examining the distribution of request types at the local level, city managers can proactively tailor services, outreach efforts, or enforcement strategies to meet the unique needs of each neighborhood. This enables the city to shift from reactive service delivery to more targeted and preventive approaches.


**Strong Data Structure Supports Scalable Analysis and Decision-Making**

The structure of this visualization—breaking down service requests by local area and request type—indicates a well-organized and partitioned data model. This setup allows for efficient querying, filtering, and drill-down capabilities, enabling dynamic dashboards and in-depth reporting. It facilitates strategic planning by allowing departments to analyze performance metrics, identify seasonal trends, and compare geographic service patterns. The ability to track and segment this data at such a granular level positions the city to leverage predictive analytics, automate alerts, and design data-driven service improvements across departments.


# 4-	Customer Segmentation

Customer segmentation, in the context of municipal service requests, involves categorizing different types of requests based on frequency, geography, timing, and the responsible departments. This approach enables city managers and analysts to better understand patterns in citizen engagement and to tailor responses and resources accordingly.

One of the key segmentation strategies applied was by frequency of service requests. The analysis revealed that the most commonly submitted service request was for “Vegetation Encroachment on City Property,” followed closely by “Abandoned Non-Recyclables – Small Cases.” These high-volume categories signal recurring issues that likely require prioritized resource allocation, ongoing monitoring, and possibly preventive community outreach or policy adjustments.

Segmentation by geographic location uncovered trends across neighborhoods, with Downtown Vancouver registering the highest number of requests—particularly for abandoned small non-recyclable items. This pattern is likely influenced by the dense urban setting, elevated population flow, and higher visibility of public issues. Understanding these local area trends is essential for deploying targeted clean-up operations or awareness campaigns in zones with elevated need.

In terms of time-based segmentation, the dataset supports partitioning by various timeframes—year, quarter, month, and day. This structure facilitates the identification of seasonal or cyclical trends. For example, reports of vegetation encroachment typically rise during the spring and summer when plant growth accelerates. In contrast, requests involving abandoned items tend to spike during post-holiday periods or around the end of rental cycles, reflecting behavioral patterns associated with moving and waste disposal.

Finally, service requests were also segmented by department, using aggregation functions based on the ‘department’ field. Although the specific department names were not provided in the summary, the system’s configuration allows for grouping and analyzing requests to determine which departments consistently receive the highest volume of cases. This segmentation supports workforce planning, performance tracking, and inter-departmental coordination by highlighting areas of high operational demand.


Assess Request Closure Times Across Segments
The dataset includes:
Service_request_open_timestamp
Service_request_close_date
These fields are used to assess request lifecycle duration. Though exact closure time stats aren't shown, the document indicates that the data is available and structured to calculate average or median closure times—which can highlight inefficiencies or delays.


# 5-	Insights and Findings

**High-Frequency Service Request Types**

The analysis of service request records highlighted “Vegetation Encroachment on City Property” as the most frequently reported issue. This category encompasses a range of concerns such as overgrown hedges, tree branches obstructing signage, and plants extending onto sidewalks. These issues can significantly impact pedestrian visibility and safety, especially in high-traffic areas or zones with limited mobility access. The prevalence of these requests suggests a need for more frequent maintenance schedules or community outreach on property boundary responsibilities. Closely following in frequency are reports related to “Abandoned Non-Recyclable Small Items.” This includes discarded furniture, mattresses, electronics, and miscellaneous debris that have been left in public spaces. These issues not only affect the aesthetics of urban environments but also reflect broader challenges in waste management, including gaps in public education or enforcement around proper disposal methods.

**Geographic Distribution of Requests**

The geographic analysis of service requests revealed a strong spatial concentration, with Downtown Vancouver recording the highest volume of reports, particularly related to abandoned items. This trend is consistent with expectations, given the area’s high population density, intense commercial activity, and elevated levels of public foot traffic. In contrast, other neighborhoods reported relatively fewer issues, which may not necessarily indicate fewer problems. Rather, it could suggest underreporting or limited public awareness of the 3-1-1 service reporting system. Understanding this geographic disparity can inform targeted outreach efforts or resource allocation to improve engagement and ensure equitable service coverage across all communities.

**Temporal Patterns in Request Types**

Although the dataset does not explicitly contain labeled seasonal indicators, the system architecture allows for the segmentation of requests by year, quarter, month, and day. This capability enables temporal trend analysis that can reveal patterns associated with particular times of the year. For instance, vegetation-related complaints are likely to surge during the spring and summer months when plant growth is at its peak. Similarly, abandoned item reports may see seasonal spikes aligned with student move-out periods, holiday seasons, or at the end of monthly leasing cycles when renters relocate. These time-based insights can help inform proactive service planning, such as deploying additional cleanup crews during peak seasons or running public awareness campaigns in anticipation of predictable surges.

**Service Efficiency and Request Closure Times**

By leveraging timestamps for both when a service request is opened and when it is closed, the system enables detailed tracking of resolution times. Analyzing these durations across different dimensions such as by request type, neighborhood, or responsible department—can expose inefficiencies or service delivery bottlenecks. For example, consistently long closure times in certain zones may indicate resource constraints or procedural delays, while specific request types might inherently require more time due to complexity or inter-departmental coordination. Identifying such patterns is essential for prioritizing improvements, optimizing workflows, and enhancing accountability within municipal operations.

**Robust Data Infrastructure Enables Scalable Analysis**

The underlying data infrastructure, built on a suite of AWS services including Amazon S3, AWS Glue, Athena, and DataBrew, forms a robust and scalable platform for managing and analyzing service request data. This cloud-native architecture enables secure storage and real-time updates, while automating key processes such as data cleaning, profiling, and transformation. The system is designed to produce structured and consistent outputs suitable for both system-level dashboards and user-facing analytical tools. This setup not only supports current reporting needs but also provides the scalability and flexibility required for future enhancements, such as predictive modeling or integration with geospatial and IoT data sources.


# 6-	Recommendations

**Strategic Resource and Inventory Management**
   
Based on the findings that vegetation encroachment and abandoned non-recyclable small items are the most frequently reported service requests, it is essential to optimize the allocation of tools, equipment, and maintenance personnel. The city should consider proactively managing inventories such as pruning tools, garbage collection equipment, and signage in areas with historically high service volumes, especially Downtown Vancouver. Additionally, leveraging seasonal and timestamp data will help forecast peak service periods, enabling advanced planning for resource stocking and workforce scheduling. This will reduce emergency dispatches and improve the city’s ability to respond quickly to recurring issues, leading to cost savings and better public satisfaction.

 
 **Geo-Targeted Public Awareness Campaigns**
 
The data shows a significant clustering of service requests in specific areas, such as Downtown, which faces frequent reports of abandoned items. This calls for tailored, localized campaigns to raise awareness about civic responsibilities, proper waste disposal, and how to use the 3-1-1 service efficiently. Using targeted communication channels such as digital ads, posters, community meetings, and local influencers can help reinforce messaging. Conversely, areas with lower reporting rates may not necessarily face fewer problems but might lack awareness or access to the system. Educational outreach in these neighborhoods, especially in underserved communities can promote equity in public service access and improve data reliability.



**Seasonal Promotional Campaigns**

Given the system’s ability to partition data by time (year, quarter, month, day), the city can identify and prepare for seasonal trends such as vegetation overgrowth in spring or spikes in abandoned item reports during the holidays or end-of-lease periods. Promotional campaigns aligned with these patterns—such as “Spring Clean Weeks” or “Moving Season Waste Reduction” can preemptively mitigate service requests through public cooperation. These campaigns should be timed and promoted using digital platforms, local newspapers, and neighborhood associations, encouraging citizens to take preventive action. Partnering with schools, residential buildings, and community organizations will further amplify these seasonal efforts and reduce overall service demand.


**Departmental Performance Campaigns**

As the data architecture allows for request volumes to be grouped by department, performance benchmarking can be introduced to evaluate which departments manage the highest service loads and how efficiently they close requests. Departments demonstrating strong performance should be recognized through internal acknowledgments or public campaigns that highlight successful city services. Conversely, departments facing delays or large volumes should be given targeted support, along with performance incentives or internal engagement initiatives such as “Efficiency Challenges.” These campaigns not only improve morale but also foster a culture of continuous improvement and accountability within city operations.



**Predictive Planning and Budget Justification**

The structured and clean dataset offers a unique opportunity for predictive analytics that can forecast upcoming service spikes, helping city managers make data-driven budget and planning decisions. Historical patterns, closure time analyses, and geographic clusters provide a foundation for creating predictive models to anticipate future needs. The city can use this information to advocate for additional resources where needed, reallocate budgets based on seasonal or location-based forecasts, and justify capital expenditures for technology, personnel, or infrastructure enhancements. Decision-makers will be better equipped to prioritize initiatives that align with actual service trends rather than assumptions.


**Digital Engagement and App-Based Reporting Campaigns**

As multiple request channels are available—including social media, phone, and potentially a mobile app—the city should encourage broader adoption of faster, digital-first reporting tools. Promoting the use of a 3-1-1 mobile application or web portal can streamline submissions, improve data accuracy, and enable faster triaging of issues. A campaign that highlights the convenience and speed of digital reporting could include online ads, QR codes on public signage, and social media content targeted at tech-savvy demographics. Improving the digital reporting experience also reduces administrative overhead and enhances the ability to collect consistent and structured data for ongoing analysis.


# Tools and Technologies

 **Data Storage & Management**
 
Amazon S3 – Stores raw, transformed, and curated datasets.

S3 Buckets – Logical containers used to organize data stages.

S3 Versioning – Maintains historical versions of datasets.

S3 Replication – Enables data backup across regions.

Parquet Format – Efficient columnar storage format for big data.

**Data Processing & Transformation**

AWS Glue – Serverless ETL (Extract, Transform, Load) service.

AWS Glue DataBrew – Visual tool for data cleaning and profiling.

Glue Crawlers – Automatically scan data and populate metadata catalogs

ETL Jobs – Extract, transform, and load data for analytics.

Transform Recipes – Set of rules for cleaning and reformatting data.

Schema Mapping – Aligns raw data structure for analysis.

**Data Partitioning & Organization**

Partitioning by Year/Month/Day – Organizes data for efficient querying.

Column Renaming – Standardizes data field names.

Data Filtering – Focuses analysis on relevant records only.

Data Aggregation – Summarizes values by group or time.

 
**Data Profiling & Quality**

Missing Value Handling – Fills blanks with default or frequent values.

Duplicate Detection – Identifies and removes redundant records.

Uniqueness Checks – Ensures data entries are distinct.

Completeness Checks – Validates required data is present.

Freshness Checks – Ensures data is up-to-date.


**Analytics & Querying**

Amazon Athena – Serverless SQL tool to query data directly from S3.

SQL Queries – Used to extract insights and patterns.

Grouping & Aggregation Functions – Supports data summarization.

Timestamp Conversion – Used for tracking service lifecycle.


**Visualization & Presentation**

draw.io – Used to create diagrams and process visuals.

Metrics Tables – Tabular summaries of service data.

User-Friendly Output Files – Clean formats for stakeholder review.

**Security & Compliance**

AWS Key Management Service (KMS) – Encrypts data at rest.

Bucket Encryption – Protects sensitive data.

AWS CloudTrail – Monitors access and changes for auditing.

Access Controls – Restrict unauthorized data manipulation.


**Monitoring & Optimization**

Amazon CloudWatch – Monitors bucket storage and resource usage.

CloudWatch Alarms – Alerts when thresholds are exceeded.

ETL Job Monitoring – Tracks performance and failures in workflows.

Crawler Logs – Provides feedback on schema changes and issues.


**Data Output & Reporting** 

Curated Outputs – Clean datasets for user reports.

System Outputs – Logs and files for internal processing.

Report Date Field – Used to sort and group final reports.


# Deliverables
A comprehensive report covering methodology, insights, and strategic recommendations. 

Visual dashboards to explore service requests by type, department, and area. Include filters and drilldowns for dynamic analysis.

Map-based view of service request density across neighborhoods. Highlights regional hotspots and supports field team deployment.

Charts showing service request trends over months, quarters, or seasons. Reveals patterns like seasonal spikes or long-term growth.

A quick-reference page listing key performance indicators from the data. Ideal for executive briefings and progress tracking

Overview of encryption, access control, and compliance measures used. Ensures privacy and responsible handling of service data.

Detailed workflow of how raw data is collected, cleaned, and stored. Includes architecture diagrams and toolchain explanations.

Step-by-step actions to put insights into practice. Includes owners, timelines, and estimated resource needs.




# Reference 

AWS Academy. (2022). AWS Academy Cloud Foundations. Retrieved from AWS Academy: https://awsacademy.instructure.com/courses/106917/modules 

City of Vancouver. (2025). Business licences [Dataset]. Open Data Portal. Retrieved February 21, 2025, from https://opendata.vancouver.ca/explore/dataset/business-licences/information/ 




































 

 

 

 

 

 

 



 



 

 

 



 

 

 

 


 

 

 

 

 

 

 

 

 

 

 

 

 

 

 




























