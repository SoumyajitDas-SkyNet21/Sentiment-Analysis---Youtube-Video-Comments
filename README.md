YouTube Comment Sentiment Analysis Project
Key Features
Fetching and processing comments using the YouTube API.

Filtering and cleaning non-English, empty, or emoji-only comments.

Sentiment analysis using VADER (Valence Aware Dictionary and Sentiment Reasoner).

Visualization of sentiment distribution using bar and pie charts.

Storage of analyzed data for further use.

Workflow
Step 1: Generating API Key
Obtain an API key from the Google Cloud Console for the YouTube Data API.

Use the API key securely to fetch video data and comments.

Step 2: Importing Dependencies
The following Python libraries are used:

emoji: To detect emoji-only comments.

vaderSentiment: For sentiment analysis.

google-api-python-client: To interact with the YouTube API.

langdetect: To filter English comments.

matplotlib: For visualizing sentiment distribution.

pandas: To structure and manage comment data.

Step 3: Fetching Comments
Extract the video ID and channel ID using the YouTube Data API.

Fetch the top-level comments from the specified video.

Step 4: Filtering Comments
Remove comments containing only emojis or whitespace.

Apply a threshold to consider comments where text characters constitute at least 65% of the total characters.

Step 5: Extracting English Comments
Use the langdetect library to filter comments written in English, ensuring consistent analysis.
Step 6: Sentiment Analysis
Apply VADER to classify each comment as Positive, Negative, or Neutral based on compound polarity scores:

  - Compound score ≥ 0.05: Positive.

  - Compound score ≤ -0.05: Negative.

  - Otherwise: Neutral.
Step 7: Sentiment Distribution
Count the number of comments in each sentiment category:

Positive: Viewer appreciation or satisfaction.

Negative: Criticism or dissatisfaction.

Neutral: Neutral or ambiguous responses.

Step 8: Visualizing Comments
Bar Chart: Displays the count of comments in each sentiment category.

Pie Chart: Shows the percentage distribution of sentiments.

Step 9: Storing Results
Save analyzed comments and their sentiments into a text file for record-keeping and further analysis.
