# Spotify ETL Project

This project implements an ETL (Extract, Transform, Load) pipeline for Spotify data using AWS services. It retrieves data from the Spotify API, transforms the data, and loads it into an AWS Glue Data Catalog for analysis using Athena.

## **Architecture**

![Architecture Diagram](https://github.com/dkay678/Spotify-ETL/blob/main/spotify_etl_architecture.jpg)

## **Step-by-Step Instructions**

### **1. Create an S3 Bucket**
- Name: `spotify_etl_karthik`
- Folder Structure:
spotify_etl_karthik/ ├── raw_data/ │ ├── to_be_processed/ │ ├── processed/ ├── transformed_data/

### **2. Spotify Extract Lambda Function**

#### **Steps to Create `spotifyExtractFunction`:**
1. **Lambda Function Details:**
 - Name: `spotifyExtractFunction`
 - Runtime: `Python 3.8`
 - Timeout: `1 minute 3 seconds`

2. **IAM Role:**
 - Assign an IAM Role with permissions to:
   - Read/write to S3 bucket `spotify_etl_karthik`
   - Access the Spotify API (if needed).

3. **Environment Variables:**
 - Set the following environment variables:
   - `client_id`
   - `client_secret`
 - These are required to authenticate with the Spotify API.

4. **Spotipy Layer:**
 - Add a layer containing the `spotipy` package to the Lambda function.
 - The layer must be a compressed archive (e.g., `spotipy_layer.zip`) containing `spotipy`.

5. **Trigger:**
 - Add a CloudWatch Events (EventBridge) trigger to run the function daily.

6. **Script:**
 - The script for the Lambda function is located in `spotifyExtractFunction/spotifyExtractFunction.py`.

---

### **3. Spotify Transformation and Load Lambda Function**

#### **Steps to Create `spotifyTransformationLoadFunction`:**
1. **Lambda Function Details:**
 - Name: `spotifyTransformationLoadFunction`
 - Runtime: `Python 3.8`
 - Timeout: `1 minute 3 seconds`

2. **IAM Role:**
 - Assign an IAM Role with permissions to:
   - Read/write to S3 bucket `spotify_etl_karthik`.

3. **Pandas Layer:**
 - Add a layer containing the `pandas` package to the Lambda function.
 - The layer must be a compressed archive (e.g., `pandas_layer.zip`) containing `pandas`.

4. **Trigger:**
 - Add an S3 event trigger to execute the function whenever a new file is added to `raw_data/to_be_processed/`.

5. **Script:**
 - The script for the Lambda function is located in `spotifyTransformationLoadFunction/spotifyTransformationLoadFunction.py`.

---

### **4. Glue Crawlers**

1. **Create 3 Crawlers:**
 - **Crawler Names:**
   - `spotify_album_crawler`: Points to `transformed_data/album/`.
   - `spotify_artist_crawler`: Points to `transformed_data/artist/`.
   - `spotify_song_crawler`: Points to `transformed_data/song/`.
 - **IAM Role:**
   - Assign an IAM role to the crawlers to allow read access to the S3 bucket.

2. **Run the Crawlers:**
 - Ensure the schema is correctly inferred in the Glue Data Catalog.
 - If the schema is incorrect, update it manually.

---

### **5. Query with Athena**
- Use AWS Athena to query the Glue Data Catalog.
- Perform analytical queries on the `album`, `artist`, and `song` tables.

---

## **Requirements**

### **AWS Services:**
- S3
- Lambda
- Glue
- CloudWatch Events (EventBridge)
- Athena

### **Python Packages:**
- For `spotifyExtractFunction`:
- `spotipy`
- For `spotifyTransformationLoadFunction`:
- `pandas`

### **Local Setup:**
- Install the required packages using the provided `requirements.txt` files before zipping the layer archives.

---

## **Files in Repository**

- **spotifyExtractFunction/**
- `spotifyExtractFunction.py`: Lambda script for extracting data from the Spotify API.
- `requirements.txt`: Python dependencies for Spotipy.

- **spotifyTransformationLoadFunction/**
- `spotifyTransformationLoadFunction.py`: Lambda script for transforming and loading data.
- `requirements.txt`: Python dependencies for Pandas.

- **layers/**
- `spotipy_layer.zip`: Prebuilt layer archive for Spotipy.
- `pandas_layer.zip`: Prebuilt layer archive for Pandas.

- **glue-crawlers/**
- JSON configuration files for Glue Crawlers.

- **cloudwatch/**
- EventBridge rule JSON for daily trigger.

- **README.md**: Project documentation.

---

## **Setup Instructions**

1. Clone this repository.
2. Follow the step-by-step instructions for deploying each component on AWS.
3. Use the Glue Data Catalog and Athena to analyze your Spotify data.


---
