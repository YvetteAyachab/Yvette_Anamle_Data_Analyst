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









