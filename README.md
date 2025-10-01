# Real-Time SMS Spam Detection with Spark and Streamlit

This project implements an end-to-end pipeline for classifying SMS messages as "Spam" or "Ham" in real-time. It uses Apache Spark's Structured Streaming and MLlib for data processing and machine learning, with a live dashboard built in Streamlit to visualize the results.

## System Architecture

The project follows a multi-stage architecture:

1.  **Model Training**: A Logistic Regression model is trained on the UCI SMS Spam Collection dataset using a PySpark ML Pipeline. This pipeline preprocesses the text data (Tokenizer, HashingTF, IDF) and trains the classifier. The final model is saved to disk.
2.  **Data Ingestion (Stream Simulation)**: A Python script simulates a live stream of SMS messages by writing new text files to a designated `stream_data/` directory.
3.  **Real-Time Prediction**: A Spark Structured Streaming job continuously watches the `stream_data/` directory. When a new file arrives, the job loads the pre-trained model to classify the message.
4.  **Output Sink**: The results of the classification (the original message and its predicted label) are written as CSV files to a `predictions/` directory.
5.  **Live Dashboard**: A Streamlit application runs a web server that constantly monitors the `predictions/` directory. It reads the new results, updates key metrics, and refreshes the charts and tables on the dashboard in real-time.
6.  **Public Access**: `ngrok` is used to expose the local Streamlit dashboard to the internet via a public URL.

## Technologies Used

-   **Apache Spark**: For distributed data processing, machine learning (MLlib), and structured streaming.
-   **PySpark**: The Python API for Spark.
-   **Pandas**: Used for initial data loading and manipulation within the Streamlit app.
-   **Streamlit**: To create and serve the interactive, real-time web dashboard.
-   **pyngrok**: To create a public tunnel to the local Streamlit server.
-   **Jupyter/Google Colab**: As the development environment for running the code.

## Dataset

The model is trained on the **SMS Spam Collection Data Set** from the UCI Machine Learning Repository.

-   **Link**: [https://archive.ics.uci.edu/ml/datasets/sms+spam+collection](https://archive.ics.uci.edu/ml/datasets/sms+spam+collection)
-   **Description**: The dataset contains 5,572 SMS messages in English, tagged as either "ham" (legitimate) or "spam".

## How to Run the Project

This project is designed to run in a Jupyter-like environment such as Google Colab. To run it, execute the cells in the `BDA_Project.ipynb` notebook in the following order.

### 1. Prerequisites

-   A Google Colab account or a local Jupyter environment with Python installed.
-   An `ngrok` account to get a free authentication token.

### 2. Setup and Model Training

1.  **Install Dependencies**: Run the first code cell to install `pyspark`, `findspark`, `streamlit`, and `pyngrok`.
2.  **Download Data**: Execute the second cell to download and load the dataset.
3.  **Train and Save Model**: Run the third cell. This will set up a SparkSession, define the ML pipeline, train the Logistic Regression model, and save the trained model to a directory named `spam_model`.

### 3. Launch the Live Dashboard

1.  **Write Dashboard File**: Run the fourth cell (`%%writefile dashboard.py`) to create the Python script for the Streamlit app.
2.  **Start Dashboard and Ngrok**: In the fifth cell, replace `"YOUR_NGROK_AUTHTOKEN_HERE"` with your actual `ngrok` authentication token. Running this cell will:
    -   Start the Streamlit app in the background.
    -   Create a public URL using `ngrok`.
    -   **Click the output URL to open the dashboard in a new tab.**

### 4. Start the Real-Time Prediction Stream

1.  **Create Directories**: Run the sixth cell to create the `stream_data/` and `predictions/` directories.
2.  **Start the Spark Streaming Job**: Execute the seventh cell. This will start a Spark Structured Streaming job that loads the saved model and begins watching the `stream_data/` directory for new files.

### 5. Simulate Live Data

1.  **Send Sample Comments**: Run the final cell. This will loop through a list of sample comments and write them one-by-one into the `stream_data/` directory every 3 seconds.
2.  **Observe the Dashboard**: As the files are created, the Spark job will process them, and you will see the dashboard update in real-time with new metrics and the latest classified comments.

## File Structure
