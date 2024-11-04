<div align="center">
    <h1>Home Sales</h1>
</div>


<div align="center">
    <h4>By: Tai Reagan</h4>
</div>

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/home_sales.jpg" alt="home_sales" width="700"/>
</div>

---
<a name="top"></a>
# Table of Contents

<details>
  <summary>Click To Expand</summary>

- [Analysis Overview](#analysis-overview)
- [Resources](#resources)
- [What is Caching](#what-is-caching)
- [What Is Data Partitioning](#what-is-data-partitioning)
- [Analysis](#analysis)
    - [Initial Process](#initial-process)
    - [Four Bedroom Avg Price](#four-bedroom-avg-price)
    - [Three Bed Three Bath Avg Price](#three-bed-three-bath-avg-price)
    - [Three Bed Three Bath With Two Floors Avg Price](#three-bed-three-bath-with-two-floors-avg-price)
- [Runtime](#runtime)
- [Cached Runtime](#cached-runtime)
- [Parquet Runtime](#parquet-runtime)
- [Uncache Temporary Table](#uncache-temporary-table)
</details>




# Analysis Overview 
This project aims to analyze home sales data by leveraging the capabilities of SparkSQL to derive key metrics related to home features and their influence on sale prices. By examining factors such as the number of bedrooms, bathrooms, and floors, this analysis seeks to uncover meaningful insights into how these features impact home prices. Additionally, the project explores the efficiency of data processing through caching and partitioning techniques, allowing for comparisons of runtime performance under different conditions. Through this analytical approach, it will demonstrate the impact of partitioning, caching, and efficient storage formats on processing speed and query performance in large datasets. The findings provide valuable insights into the runtime advantages of these techniques, which are essential for optimizing performance in data-intensive applications.

## Resources 
The data used for the analysis was provided by [Home Sales Data](https://2u-data-curriculum-team.s3.amazonaws.com/dataviz-classroom/v1.2/22-big-data/home_sales_revised.csv)

## What is Caching
Caching stores frequently accessed data temporarily in memory, speeding up repeated data retrievals during processing. By caching a temporary table in Spark, this analysis will compare the performance of cached vs. uncached data, observing how caching can reduce latency and improve runtime for subsequent queries. Once caching is no longer necessary, the temporary table will be uncached to free up memory resources, ensuring efficient memory management.


## What Is Data Partitioning
Partitioning and caching are fundamental techniques in big data processing, crucial for enhancing the efficiency and scalability of data analysis tasks. **Data partitioning** involves dividing large datasets into smaller, manageable sections, or "partitions," allowing Spark to process data more effectively by reducing the volume handled in each operation. This technique separates a logical database or its components into independent parts that can be stored, accessed, and managed individually. Partitioning is widely used to improve manageability, optimize performance, ensure data availability, and balance workload across the system, ultimately enhancing the speed and scalability of large-scale data processing.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/Data_Partitioning_1.jpg" alt="data_partitioning_1" width="600"/>
</div>


### The two primary types of data partitioning:
---

<div align="center">
    <h4>Horizontal Partitioning</h4>
</div>

  - This method divides the database by rows, creating partitions based on specific row criteria. For example, data may be split according to a particular attribute, such as bedroom count or year of sale.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/horizontal.png" alt="horizontal" width="600"/>
</div>


<div align="center">
    <h4>Vertical Partitioning</h4>
</div>

  - This approach divides the database by columns, where different sets of columns are stored in separate partitions. This method is useful when certain columns are accessed more frequently than others.
  

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/vertical.jpg" alt="vertical" width="600"/>
</div>

In this analysis, horizontal partitioning was applied by dividing rows of data based on specific criteria, such as filtering for particular numbers of bedrooms, bathrooms, and floors, or by grouping data by year. This method allows for efficient data handling and targeted analysis based on relevant features.


## Analysis

### Initial Process

To begin the initial process we first read in the AWS S3 bucket into a DataFrame from **pyspark** and imported **SparkFiles**.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/read_in_data.png" alt="read_in_csv" width="600"/>
</div>


### Four Bedroom Avg Price

The analysis began by determining the average sale price of four-bedroom houses for each year. Using SparkSQL, the average price was calculated, rounded to two decimal places, and grouped by year to reveal trends over time. The results offer insights into year-over-year changes in the market value of four-bedroom homes.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/4_bedroom.png" alt="4_bedroom" width="600"/>
</div>




### Three Bed Three Bath Avg Price
Next question was aimed to find the average sale price of homes with three bedrooms and three bathrooms, categorized by the year each home was built. Using SparkSQL, the average price was calculated for each construction year, rounded to two decimal places, and sorted in descending order by the year built. This provides insight into how home prices for properties with specific features have varied over time.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/3_bed_3_bath.png" alt="3_bed_3_bath" width="600"/>
</div>




### Three Bed Three Bath With Two Floors Avg Price
The last question calculates the average sale price for homes that meet the specified criteria (three bedrooms, three bathrooms, two floors, and at least 2,000 square feet) for each construction year. Using SparkSQL, the average price was computed, rounded to two decimal places, and grouped by the year built in descending order. This provides insight into the annual variations in market value for these specific types of homes.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/3_bed_3_bath_2_floors.png" alt="3_bed_3_bath_2_floors" width="600"/>
</div>



### Runtime
This step calculates the average sale price of homes based on their "view" rating, specifically for homes with an average price less than $350,000. Using uncached data, SparkSQL performs the query to group data by view rating, calculate the average price for each group (rounded to two decimal places), and filter for those below the specified price threshold. The runtime for this query is measured to provide a baseline for comparison with subsequent cached queries, allowing for an evaluation of caching’s impact on query performance.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/runtime.png" alt="runtime" width="600"/>
</div>



### Cached Runtime
At this stage, a temporary table "home sales" has been cached. Using this cached data, the previous query is re-run to calculate the average price of homes based on their "view" rating, specifically for those with an average price below $350,000. The runtime for the cached query is measured and compared to the uncached runtime, providing insights into the efficiency gains achieved through caching.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/cached_runtime.png" alt="cached_runtime" width="600"/>
</div>

<div align="center">
    <h4>Uncached Runtime vs Cached Runtime</h4>
</div>

After executing the query on the cached data, the cached runtime was compared to the original uncached runtime, revealing that the cached runtime of 0.633 seconds was significantly faster than the uncached runtime of 1.018 seconds.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/cached_vs_runtime.png" alt="cached_vs_runtime" width="600"/>
</div>



### Parquet Runtime


The final step involved comparing the runtime differences between cached and partitioned data. To achieve this, the home sales data was first partitioned by the "date_built" field and saved in Parquet format. A temporary table was then created from the partitioned Parquet data to facilitate efficient querying and performance comparison.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/partition_data.png" alt="partition_data" width="600"/>
</div>


The final query was executed to calculate the average price of a home based on its "view" rating, specifically for homes with an average price below $350,000. The runtime for this query, using the partitioned Parquet data, was recorded to allow comparison with the uncached runtime, providing insight into performance improvements from partitioning.
<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/parquet_runtime.png" alt="parquet_runtime" width="600"/>
</div>

<div align="center">
    <h4>Cached Runtime vs Parquet Runtime</h4>
</div>

After executing the query on the partitioned Parquet data, a comparison with the cached runtime showed that the Parquet runtime of 0.702 seconds was slightly higher than the cached runtime of 0.633 seconds.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/parquet_vs_cached.png" alt="parquet_vs_cached" width="600"/>
</div>

### Uncache Temporary Table

To conclude the analysis, the **home_sales** temporary table was uncached, and its uncached status was verified using **PySpark** to ensure that it was successfully removed from memory.

<div align="center">
    <img src="https://github.com/Taireagan/Home-Sales/blob/main/Images/closing_cached.png" alt="closing_cached" width="600"/>
</div>
