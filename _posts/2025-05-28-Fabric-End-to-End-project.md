---
layout: post
title: "Sustainable Energy Access: An End-to-End Project in Microsoft Fabric"
subtitle: "From World Bank API ingestion to a star schema model and interactive Power BI dashboard"
background: '\img\posts\fabric_end_to_end\nicholas-doherty-pONBhDyOFoM-unsplash.jpg'
---

*<a href="https://unsplash.com/photos/white-electic-windmill-pONBhDyOFoM?utm_content=creditShareLink&utm_medium=referral&utm_source=unsplash">Photo</a> by <a href="https://unsplash.com/@nrdoherty">Nicholas Doherty</a> on <a href="https://unsplash.com">Unsplash</a>



# SE4ALL: Powering Sustainable Development Goals with Data
by Estitxu Larralde

      
This project showcases a comprehensive data pipeline and visualization journey fully implemented in Microsoft Fabric, using data from the World Bank SE4ALL (Sustainable Energy for All) initiative. It explores how access to sustainable energy evolves over time and across regions—covering metrics like electricity access, renewable energy usage, and clean cooking fuel adoption. From ingesting the raw JSON files to building a relational model and publishing an interactive report, this project highlights the power of Fabric for end-to-end data workflows.


## Table of Contents
<ul>
<li><a href="#datasource">Data Source</a></li>
<li><a href="#workflow">Workflow Overview</a></li>
 <ul>
      <li><a href="#pipeline">Data ingestion using Microsoft Fabric pipelines and API request</a></li>
      <li><a href="#transformation">Data Transformation with PySpark notebooks and Modeling</a></li>
      <li><a href="#powerbi">Building the Power BI report</a></li>
    </ul>
<li><a href="#challenge">Technical Challenges and Limitations</a></li>
<li><a href="#conclusions">Conclusions</a></li>
</ul>


<a id='datasource'></a>

### Dataset Source


The dataset was extracted via the <a href="https://data360.worldbank.org/en/api">PhotoWorld Bank Data360 API</a>, focusing on SE4ALL indicators. The SE4ALL initiative, launched in 2010 by the UN Secretary General, established three global objectives to be accomplished by 2030:

*	Ensure universal access to modern energy services

*	Double the global rate of improvement in energy efficiency

*	Double the share of renewable energy in the global energy mix

The SE4ALL database supports this initiative by providing historical country-level data on electricity access, clean cooking fuel adoption, renewable energy share by technology, and improvements in energy intensity.
In total, over 141,000 rows were extracted. In addition to the main dataset, several reference tables (for countries, indicators, energy source composition, etc.) were imported from CSV files.


<a id='workflow'></a>

### Workflow Overview

<a id='pipeline'></a>

#### 1. Data Ingestion using Microsoft Fabric's Pipeline to connect to World Bank's API

A Fabric pipeline was built to handle paginated API requests:
*	A parameterized ForEach loop triggered repeated API calls.

![png](\img\posts\fabric_end_to_end\Pipeline_1.png)

*	Results were stored as JSON files in our lakehouse.

![png](\img\posts\fabric_end_to_end\Json_pages.png)

*	Lookup tables (CSV files) were uploaded into Fabric.

![png](\img\posts\fabric_end_to_end\Upload_csv.png)


<a id='transformation'></a>

#### 2. Data Transformation & Modeling

Using **PySpark notebooks**, the raw JSON data was transformed:
*	Exploded the nested value field into individual rows.
*	Cleaned missing or invalid entries.
*	Created additional columns: year (as integer), date (as date), area type (country vs region), etc.

![png](\img\posts\fabric_end_to_end\notebook.png)


We designed a **star schema**:
*	One fact table: se4all_cleaned
*	Five dimension tables: dim_area, dim_indicator, dim_urbanisation, dim_composition, dim_date_clean

![png](\img\posts\fabric_end_to_end\Semantic_model.png)

This structure enables intuitive and efficient data exploration through Power BI.

<a id='powerbi'></a>

#### 3. Building the Power BI Report

A fully interactive Power BI report was developed within the Fabric environment, without needing Power BI Desktop.

##### *Page 1: Overview*
*	A **line chart** comparing the average values of indicators over time for a given country or area.
*	**Slicers** for area/country, indicator, and urbanisation level.
*	Two **KPI cards**:
    * **Latest average value recorded**
    * **Growth percentage** from the first to the latest year with available data

![png](\img\posts\fabric_end_to_end\Page_1.png)


##### *Pages 2 & 3: Comparison Through Indicators*
    
    - Page 2: 
        * Two **bar charts**:
	      - Installed renewable electricity-generating capacity (watts per capita)
          - International financial flows to developing countries
        * **Slicers** for selecting multiple countries/regions and a year range (e.g. 1990–2022)

![png](\img\posts\fabric_end_to_end\Page_2.png)

    - Page 3:
        * Two **column charts**:
          - Clustered column chart for Final consumption of renewable energy (PJ)
          - Stacked column chart for Share in total final energy consumption (%)

![png](\img\posts\fabric_end_to_end\Page_3.png)


##### *Pages 4 & 5: Data Coverage and Completeness*

    - Page 4:
        * A **matrix** with areas as rows and years as columns, showing the count of reported values
        * **Cards** displaying:
           - Number of covered countries
           - Number of covered years
           - Total number of records

![png](\img\posts\fabric_end_to_end\Picture_4.png)

    - Page 5:
        * A **bar chart** of years with reported records by country/region (ascending order)
        * **Slicer** for selecting an indicator
        * **Card** showing the number of covered countries (dynamic based on the indicator selected)

![png](\img\posts\fabric_end_to_end\Picture_5.png)


<a id='challenge'></a>

### Technical Challenges and Limitations

While Fabric is powerful, a few limitations were encountered during development:
*	In the trial version, calculated columns can't be added directly in the semantic model UI.
*	Default semantic models are inflexible—relationships must be recreated in custom models.
*	Notebook-lakehouse linking must be configured carefully to ensure correct data syncing.
*   Formatting options within Power BI in Fabric are more limited than in Power BI Desktop (e.g., color palette selection)

<a id='conclusions'></a>
### 📌 Conclusions

This project demonstrates how Microsoft Fabric supports a modern, cloud-native data pipeline—from ingestion and transformation to modeling and reporting—all in one unified environment. By implementing a clean star schema, we ensured a scalable and efficient data model. The Power BI report built directly within Fabric enabled intuitive, interactive, and professional storytelling based on the dataset.

While the dataset offers rich insights into global energy access and consumption, the purpose of this project was to focus on the end-to-end technical workflow rather than conduct an in-depth exploratory or statistical analysis. 

For those interested in the findings and policy implications of this data, we recommend exploring the World Bank’s *Global Tracking Framework* report, which provides detailed analyses and global insights:

👉 [Global Tracking Framework Report – World Bank](https://www.worldbank.org/en/topic/energy/publication/Global-Tracking-Framework-Report)

🗃 View the full notebook, csv files, Power BI report in different formats, and source code in my <a href="https://github.com/Pitxunet/Fabric-end-to-end-project">GitHub repository</a>.