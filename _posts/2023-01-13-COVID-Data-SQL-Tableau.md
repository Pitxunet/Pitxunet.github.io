---
layout: post
title: "Coronavirus cases and deaths over a period of time"
subtitle: "EDA with SQL using Microsoft SQL Server Management Studio and Data Visualization with Tableau Public"
background: '/img/posts/Covid/fusion-medical-animation-rnr8D3FNUNY-unsplash.jpg'
---
Photo of <a href="https://unsplash.com/@fusion_medical_animation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Fusion Medical Animation</a> at <a href="https://unsplash.com/fr/photos/rnr8D3FNUNY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

# Covid-Portfolio-project
EDA with SQL using Microsoft SQL Server Management Studio and creating a Tableau Dashboard with multiple visualizations.

## Dataset

The dataset used for this project was downloaded from <a href="https://ourworldindata.org/covid-deathst">Our World in Data</a>.

It's been divided in two separated Excel files to create two separate tables and link them using SQL queries on Microsoft SQL Server Management Studio.


## Purpose

The purpose is to explore the data on Covid deaths and cases to get a global vue while also focusing in the figures of countries of interest (France - as it's my country of residence, China, India, USA, UK).

Microsoft SQL Server Management Studio is a nice tool to create a database, add tables, and create SQL queries.

![jpg](\img\posts\Covid\ssms-EDA-queries.JPG)

## Sql views

After the exploratory queries on the data, I created a few SQL views. For example:

![jpg](\img\posts\Covid\ssms-EDA-view.JPG)

The different views can be seen in my Github repository <a href="https://github.com/Pitxunet/Covid-Portfolio-project/blob/main/Portfolio-SQL-Project-COVID.sql">Covid-portfolio-project</a>

## Tableau

Then I selected some of them to create a simple Dashboard in Tableau Public.

First I copied the results from a selection of queries/views into Excel. I uploaded each Excel sheet as a different table to Tableau Public. I created visualizations that in my opinion would be nice to get together in a dashboard.

Finally, I published the resulting Dashboard into Tableau Public.

![jpg](\img\posts\Covid\Tableau-Covid.JPG)

You can have a look at and play with this Dashboard at my <a href="https://public.tableau.com/app/profile/estitxu.larralde.erasun/viz/CovidDashboard_16720892806330/Dashboard1">Tableau Public Profile</a>.

