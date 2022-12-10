---
layout: post
title: "Airbnb locations map in Amsterdam"
subtitle: "Wrangling a dataset with numpy and creating an app in Streamlit"
background: '/img/posts/airbnb/buildings-6778915_1920.jpg'
---

Based on a dataset of Airbnb locations in Amsterdam and its coordonates, I calculated the distance to a touristical attraction (my favourite spot in Amsterdam: Leidseplein).

First I uploaded the cvs datafile into a Google Colab document. Then I used numpy library to change the currency, update prices as per inflation and calculate the distance between the locations and my favourite spot.

Finally, I deployed an app with Streamlit, which includes the interactive version of the map below. The app can be seen <a href="https://pitxunet-amsterdam-airbnb-locations-app-streamlit-app-o69iri.streamlit.app/">here</a> 


![jpg](\img\posts\airbnb\Amsterdam.JPG)