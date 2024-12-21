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
Install dependencies:

Install all required libraries using pip:
bash
Copy code
pip install -r requirements.txt
Obtain a YouTube API Key:

Follow the instructions in Google's YouTube API documentation to create a project and get your YouTube API key.
Set up the YouTube API Key:

Replace 'YOUR_API_KEY' in the app.py file with your actual YouTube API key.
Run the Streamlit App:

bash
Copy code
streamlit run app.py
Use the app:

Open the app in your browser (usually at http://localhost:8501).
Paste a YouTube video URL into the input box and click "Analyze".
Deployment on Streamlit Cloud
Push the code to GitHub (if not done already).
Go to Streamlit Cloud, log in with your GitHub account, and deploy the app from your repository.
Technologies Used
Python: Programming language used for data analysis and web app.
Streamlit: Framework for creating interactive web apps.
YouTube Data API v3: For fetching YouTube video comments.
VADER Sentiment Analysis: For analyzing the sentiment of text.
Matplotlib: For generating charts and graphs.
WordCloud: For generating word clouds from comments.
Dependencies
To run this project, you need the following Python libraries:

streamlit
google-api-python-client
vaderSentiment
matplotlib
wordcloud
emoji
langdetect
To install all dependencies, run the following command:

bash
Copy code
pip install -r requirements.txt
How the Project Works
Fetch Comments: The app fetches up to 600 comments from a YouTube video by extracting the video ID from the URL.
Sentiment Analysis: It then analyzes the sentiment of each comment using the VADER SentimentIntensityAnalyzer, classifying them into positive, negative, or neutral categories.
Data Visualization: The sentiment distribution is visualized using:
Bar Chart
Pie Chart
Donut Chart
Word Cloud of the most frequent words in the comments.
Streamlit Front-End: The user enters a YouTube URL, and by clicking the "Analyze" button, the app fetches the comments and displays the sentiment analysis results in graphical formats.
Screenshots
You can add screenshots of the app in action to showcase the results. For example:

(Replace with your actual screenshot)

License
This project is licensed under the MIT License - see the LICENSE file for details.

Contact
For any questions or suggestions, feel free to reach out to [Your Name] at [Your Email].

Troubleshooting Tips
No comments fetched: Ensure that the video URL is correct and public. Private or restricted videos might not return comments.
API Key errors: Make sure the YouTube API key is valid and has the necessary permissions. If you hit the API quota limit, try again later.
Additional Notes
This project is meant for educational purposes and could be extended with additional features such as filtering by sentiment, providing a more detailed analysis, etc.
markdown
Copy code

### 3. **Save the README.md**:
After pasting the content into the `README.md` file, save the file. 

### 4. **Push the README File to GitHub**:

If you haven't yet pushed your changes to GitHub, you can follow these steps:

1. **Open your terminal or command prompt** and navigate to your project directory.
2. **Stage and commit the changes** (if your README.md file is new or modified):
   ```bash
   git add README.md
   git commit -m "Add detailed README file"
Push the changes to GitHub:
bash
Copy code
git push origin main
(Make sure to replace main with your current branch name if it's different.)
