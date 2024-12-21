# YouTube Comment Sentiment Analysis

This project performs sentiment analysis on YouTube video comments, allowing users to analyze the overall sentiment (positive, negative, or neutral) of the comments from a given YouTube video URL. It uses the YouTube Data API to fetch comments and the **VADER Sentiment Analysis** algorithm to classify the sentiment.

## **Features**
- **Fetch YouTube comments**: Collects comments from a given YouTube video URL.
- **Sentiment Analysis**: Analyzes comments and categorizes them into positive, negative, or neutral.
- **Data Visualization**: Displays sentiment distribution as:
  - Bar Chart
  - Pie Chart
  - Donut Chart
  - Word Cloud
- **Streamlit Front-End**: A simple interface where users can paste a YouTube video URL and click the "Analyze" button to start the analysis.

## **How to Use**

### **Prerequisites**
- Python 3.x
- YouTube Data API Key

### **Steps to Run Locally**
1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-username/your-repository.git
   cd your-repository
