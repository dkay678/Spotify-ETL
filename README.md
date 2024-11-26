# Spotify ETL Project

This project implements an ETL (Extract, Transform, Load) pipeline for Spotify data using AWS services. It retrieves data from the Spotify API, transforms the data, and loads it into an AWS Glue Data Catalog for analysis using Athena.

## **Architecture**

![Architecture Diagram](images/architecture_diagram.png)

## **Step-by-Step Instructions**

### **1. Create an S3 Bucket**
- Name: `spotify_etl_karthik`
- Folder Structure:
spotify_etl_karthik/ ├── raw_data/ │ ├── to_be_processed/ │ ├── processed/ ├── transformed_data/
