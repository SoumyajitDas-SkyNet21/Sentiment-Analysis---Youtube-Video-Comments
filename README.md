# YouTube Comment Sentiment Analysis Project

## Introduction

- The YouTube Comment Sentiment Analysis project is designed to analyze the sentiments expressed in comments on a specific YouTube video. The goal is to classify comments into three categories: Positive, Negative, and Neutral. This analysis provides insights into the overall audience reaction, helping creators, marketers, and researchers better understand public opinion.

## Purpose of the Project

- **Audience Insights**: Understand how viewers respond emotionally to content.

- **Feedback Analysis**: Extract meaningful feedback for content improvement.

- **Community Moderation**: Identify and address harmful or overly negative comments.

- **Marketing Insights**: Assess sentiment trends for campaigns or videos.

## Key Features

- Fetching and processing comments using the YouTube API.

- Filtering and cleaning non-English, empty, or emoji-only comments.

- Sentiment analysis using VADER (Valence Aware Dictionary and Sentiment Reasoner).

- Visualization of sentiment distribution using bar and pie charts.

- Storage of analyzed data for further use.

## Workflow

- ### Step 1: Generating API Key

    - Obtain an API key from the Google Cloud Console for the YouTube Data API.

    - Use the API key securely to fetch video data and comments.
 
- ### Step 2: Importing Dependencies

  The following Python libraries are used:

    - emoji: To detect emoji-only comments.

    - vaderSentiment: For sentiment analysis.

    - google-api-python-client: To interact with the YouTube API.

    - langdetect: To filter English comments.

    - matplotlib: For visualizing sentiment distribution.

    - pandas: To structure and manage comment data.
 
- ### Step 3: Fetching Comments

    - Extract the video ID and channel ID using the YouTube Data API.

    - Fetch the top-level comments from the specified video.
 
- ### Step 4: Filtering Comments

    - Remove comments containing only emojis or whitespace.

    - Apply a threshold to consider comments where text characters constitute at least 65% of the total characters.

- ### Step 5: Extracting English Comments

    - Use the langdetect library to filter comments written in English, ensuring consistent analysis.

- ### Step 6: Sentiment Analysis

    - Apply VADER to classify each comment as Positive, Negative, or Neutral based on compound polarity scores:

            - Compound score ≥ 0.05: Positive.

            - Compound score ≤ -0.05: Negative.

            - Otherwise: Neutral.

- ### Step 7: Sentiment Distribution

    - Count the number of comments in each sentiment category:

        - Positive: Viewer appreciation or satisfaction.

        - Negative: Criticism or dissatisfaction.

        - Neutral: Neutral or ambiguous responses.

- ### Step 8: Visualizing Comments

    - Bar Chart: Displays the count of comments in each sentiment category.

    - Pie Chart: Shows the percentage distribution of sentiments.

- ### Step 9: Storing Results

    - Save analyzed comments and their sentiments into a text file for record-keeping and further analysis.

#### Importing necessary dependencies 


```python
# For Fetching Comments
from googleapiclient.discovery import build
# For filtering comments 
import re
# For filtering comments with just emojis
import emoji
# Analyze the sentiments of the comment
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
# For Visualization 
import matplotlib.pyplot as plt 
```

#### Fetching the video id & channel id 


```python
# Storing the api in API_KEY variable
API_KEY='AIzaSyAKylkn9ANwkb2gA1q8MEVpMrs_a_UbHJY'

# Initializing Youtube API 
youtube=build('youtube','v3',developerKey=API_KEY)

video_id=input("Enter Youtube Video URL : ")[-11:]
print("Video Id: "+ video_id)

# Getting the channelId of the video uploader 
video_response=youtube.videos().list(
                part='snippet',
                id=video_id).execute()

# Splitting the response for channelId
video_snippet=video_response['items'][0]['snippet']
uploader_channel_id=video_snippet['channelId']
print("Channel Id: "+uploader_channel_id)


               


```

    Enter Youtube Video URL :  https://www.youtube.com/watch?v=pSeaHfkd3M8
    

    Video Id: pSeaHfkd3M8
    Channel Id: UCDPM_n1atn2ijUwHd0NNRQw
    

#### Fetching Comments 


```python
# Fetch comments 
print("Fetching Comments...")
comments=[]
nextPageToken=None
while len(comments) <600:
    request=youtube.commentThreads().list(
        part='snippet',
        videoId=video_id,
        maxResults=100,
        pageToken=nextPageToken )

    response=request.execute()
    for item in response['items']:
        comment=item['snippet']['topLevelComment']['snippet']
        # Check if the comment is not from video uploader 
        if comment['authorChannelId']['value']!=uploader_channel_id:
            comments.append(comment['textDisplay'])
        nextPageToken=response.get('nextPageToken')

        if not nextPageToken:
            break
    # Printing 5 comments 
comments[:10]
```

    Fetching Comments...
    




    ['❤',
     'Hopeful to find love that makes me feel like this song🥹',
     'My dad passed away this year and Coldplay was his favorite. This feels like him giving us a message to get through this christmas period without him, love you dad!',
     'Awesome tribute to a awesome actor!',
     'Who else can NOT listen to this 5 days before Christmas!? Beautiful, but so sad! 😢I miss my mum! 2-13-22, it changed me forever!',
     '❤️❤️❤️❤️❤️❤️❤️❤️❤️❤️❤️',
     'Coldplay,s every song is just Masterpiece🥰💖✨❤',
     '❤❤❤❤',
     '👋',
     'He still got his groove at 99! Has such a mischievous smile and looks cool dancing']



#### Filtering Comments 


```python
hyperlink_pattern = re.compile(
    r'http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\\(\\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+')

threshold_ratio=0.75
relevant_comments=[]
# Inside your loop that processes comments 

for comment_text in comments:
    comment_text=comment_text.lower().strip()
    emojis=emoji.emoji_count(comment_text)

    # Count text characters (exclusinding Spaces)
    text_characters=len(re.sub(r'\s','',comment_text))
    if (any(char.isalnum() for char in comment_text)) and not hyperlink_pattern.search(comment_text):
        if emojis == 0 or (text_characters / (text_characters + emojis)) > threshold_ratio:
            relevant_comments.append(comment_text)
# Print the relevant comments
relevant_comments[:10]
```




    ['hopeful to find love that makes me feel like this song🥹',
     'my dad passed away this year and coldplay was his favorite. this feels like him giving us a message to get through this christmas period without him, love you dad!',
     'awesome tribute to a awesome actor!',
     'who else can not listen to this 5 days before christmas!? beautiful, but so sad! 😢i miss my mum! 2-13-22, it changed me forever!',
     'coldplay,s every song is just masterpiece🥰💖✨❤',
     'he still got his groove at 99! has such a mischievous smile and looks cool dancing',
     'i haven&#39;t heard a recent coldplay song that sounds like the way it sounded on their four albums. i knew it was time to listen to this style again.',
     'got tears listening to this song',
     '🥰forever....',
     'omg is he&#39;s the actor in  mary poppins?']



#### Filtering only English Comments 


```python
import re
import emoji
from langdetect import detect, DetectorFactory, LangDetectException

# To make the results deterministic
DetectorFactory.seed = 0

# Hyperlink pattern to filter out links
hyperlink_pattern = re.compile(
    r'http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\\(\\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+')

threshold_ratio = 0.65
relevant_comments = []

# Loop to process comments
for comment_text in comments:
    comment_text = comment_text.lower().strip()

    # Skip comments containing hyperlinks
    if hyperlink_pattern.search(comment_text):
        continue

    # Count emojis and text characters (excluding spaces)
    emojis_count = emoji.emoji_count(comment_text)
    text_characters = len(re.sub(r'\s', '', comment_text))

    # Check if the comment is predominantly text and not just emojis
    if any(char.isalnum() for char in comment_text):
        if emojis_count == 0 or (text_characters / (text_characters + emojis_count)) > threshold_ratio:
            try:
                # Detect language and filter for English comments
                if detect(comment_text) == 'en':
                    relevant_comments.append(comment_text)
            except LangDetectException:
                # Handle errors for comments that can't be processed by langdetect
                continue

# Print the first 5 relevant comments
print(relevant_comments[:50])

```

    ['hopeful to find love that makes me feel like this song🥹', 'my dad passed away this year and coldplay was his favorite. this feels like him giving us a message to get through this christmas period without him, love you dad!', 'awesome tribute to a awesome actor!', 'who else can not listen to this 5 days before christmas!? beautiful, but so sad! 😢i miss my mum! 2-13-22, it changed me forever!', 'coldplay,s every song is just masterpiece🥰💖✨❤', 'he still got his groove at 99! has such a mischievous smile and looks cool dancing', 'i haven&#39;t heard a recent coldplay song that sounds like the way it sounded on their four albums. i knew it was time to listen to this style again.', 'got tears listening to this song', 'omg is he&#39;s the actor in  mary poppins?', 'yayyyyy!!!!! i got here before it went viral ! 🥰😋🙏🏾⚡🫶🏾💪🏾', 'touch deeply inside me..whoever read this comment,you got all my love.wish we can be a better human for this world❤', 'just when you thought the lyric video couldn&#39;t get any better.', 'i&#39;m leaving this comment here so after a month or a year, when someone like it, i get reminded of this masterpiece.🔝👌', 'this song breaks your heart because it shows how short life is! 1 million blinks and it&#39;s over!', 'all my love for palestine', 'lucky to have so much family', 'awwww 99 + red balloons in the end', 'my favorite band i grew up with such a great man chris your so awesome', 'wow , such a tear jerking song / video . ❤❤❤❤simply beautiful.', 'been a long time since i’ve teared up watching a music video 😊❤', 'a beautiful tribute to a beautiful soul. coldplay hit it out of the park. thanks for the memories.', 'yes! this is what im talking about!  🥹🙌', 'i just know imraan is listening to this.', 'whats  the big diff we old cats know who this is young ones no   do a song for kate hepburn  . tracy😊', 'just the sound of his voice makes you smile. &lt;3', 'an emotional masterpiece', 'that was beautiful, well done 🥰🥰🥰', 'this is the best thing iv ever seen ❤❤❤ very emotional stuff huge respect ❤❤❤my heart is some what full thank u chris and sir van dyke', 'what is love??? it&#39;s loving someone more than you love yourself.', 'thanks for writing such an amazing song! i made a cover for my wife for this christmas, hope she likes it!', 'this is beautiful ❤', 'happy 99th birthday mr van dyke! ❤️ what a men! still a magnificent dancer..and his eyes are so full of happiness and joy.. ❤️ i cry everytime watching this 😭♥️ thanks chris, thanks coldplay for this stunning song and this precious tribute to a living true legend ♥️', 'who cut onions 😢❤', 'aweee my heart!!', 'coldplay came to close the year with a flourish ✨', 'i love that he’s receiving his flowers while he’s here', 'what a fantastic way to celebrate a wonderful man.happy 99th sir.', 'oh, for now and always, till the end of my days<br>you got all my love<br>you&#39;ve got all my love<br><br>can&#39;t be any better 😊', 'now i see this on 43 inch i see he is the same man', 'oh my goodness... these are not tears, they are memories. what a life he&#39;s lived! what a work of art this is! soon to be 70 and this is a sweet reminder of mr. van dyke and others we&#39;ve loved over the years. many gone now, and some of the best still with us.    ...chris martin&#39;s talent is magical! 🧡💛', 'coldplay this is hot !❤', 'two legends in a video.<br>❤️', 'all wonderful 🌈✨', 'amazing to see a great old actor recognised in a modern twist....top marks chris and the boys..keep it up', 'magnificent.  love is our only goal in our life, please never forget it. thank coldplay for reminding us ❤', 'i am gandhi...the world is my. family ..we must spread love....to be a part of family  because our life is too short to argue thankyou team of cold play<br>to create oness in this world', 'honestly, i think coldplay still has the perfect formula to make their music a hit.', 'this is poetic! 🤩', 'sweet song with the sweetest of characters. ❤', '❤ my heart is so 💓']
    


```python

```

#### Storing comments in a file for further access


```python
f=open("YoutubeComments.txt","w",encoding="utf-8")
for idx, comment in enumerate(relevant_comments):
    f.write(str(comment)+"\n")
f.close()
print("Comments Stored Successfully !")
```

    Comments Stored Successfully !
    

#### Analyzing Comments 


```python
def sentiment_scores(comment, polarity):

    # Creating a SentimentIntensityAnalyzer object.
    sentiment_object = SentimentIntensityAnalyzer()

    sentiment_dict = sentiment_object.polarity_scores(comment)
    polarity.append(sentiment_dict['compound'])

    return polarity


polarity = []
positive_comments = []
negative_comments = []
neutral_comments = []

f = open("YoutubeComments.txt", 'r', encoding='`utf-8')
comments = f.readlines()
f.close()
print("Analysing Comments...")
for index, items in enumerate(comments):
    polarity = sentiment_scores(items, polarity)

    if polarity[-1] > 0.05:
        positive_comments.append(items)
    elif polarity[-1] < -0.05:
        negative_comments.append(items)
    else:
        neutral_comments.append(items)

# Print polarity
polarity[:5]
```

    Analysing Comments...
    




    [0.875, 0.5892, 0.8588, -0.8951, 0.9758]



#### Overall Polarity


```python
avg_polarity = sum(polarity)/len(polarity)
print("Average Polarity:", avg_polarity)
if avg_polarity > 0.05:
    print("The Video has got a Positive response")
elif avg_polarity < -0.05:
    print("The Video has got a Negative response")
else:
    print("The Video has got a Neutral response")

print("The comment with most positive sentiment:", comments[polarity.index(max(
    polarity))], "with score", max(polarity), "and length", len(comments[polarity.index(max(polarity))]))
print("The comment with most negative sentiment:", comments[polarity.index(min(
    polarity))], "with score", min(polarity), "and length", len(comments[polarity.index(min(polarity))]))
```

    Average Polarity: 0.5433824615384616
    The Video has got a Positive response
    The comment with most positive sentiment: this is the best thing iv ever seen ❤❤❤ very emotional stuff huge respect ❤❤❤my heart is some what full thank u chris and sir van dyke
     with score 0.9909 and length 135
    The comment with most negative sentiment: this song made me cry.. i remember my late father. i&#39;m by his side lying on the hospital bed when he died..😢😢😢
     with score -0.9607 and length 115
    

#### Visualizing Comments 

### Bar Chart


```python
positive_count = len(positive_comments)
negative_count = len(negative_comments)
neutral_count = len(neutral_comments)

# labels and data for Bar chart
labels = ['Positive', 'Negative', 'Neutral']
comment_counts = [positive_count, negative_count, neutral_count]

# Creating bar chart
plt.bar(labels, comment_counts, color=['green', 'red', 'grey'])

# Adding labels and title to the plot
plt.xlabel('Sentiment')
plt.ylabel('Comment Count')
plt.title('Sentiment Analysis of Comments')

# Displaying the chart
plt.show()
```


    
![png](Youtube%20Comment%20Sentiment%20Analysis_files/Youtube%20Comment%20Sentiment%20Analysis_24_0.png)
    



```python
plt.figure(figsize=(10, 6))
colors = ['#76c7c0', '#f76c6c', '#ffcc5c']  # Custom colors
plt.bar(labels, comment_counts, color=colors, edgecolor='black', linewidth=1.2)

# Adding data labels
for i, value in enumerate(comment_counts):
    plt.text(i, value + 1, str(value), ha='center', fontsize=12, fontweight='bold')

plt.title('Sentiment Distribution', fontsize=16, fontweight='bold')
plt.xlabel('Sentiments', fontsize=14)
plt.ylabel('Number of Comments', fontsize=14)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()

```


    
![png](Youtube%20Comment%20Sentiment%20Analysis_files/Youtube%20Comment%20Sentiment%20Analysis_25_0.png)
    


### Pie Chart 


```python
# labels and data for Bar chart
labels = ['Positive', 'Negative', 'Neutral']
comment_counts = [positive_count, negative_count, neutral_count]

plt.figure(figsize=(10, 6)) # setting size

# plotting pie chart
plt.pie(comment_counts, labels=labels,shadow=True,radius=.90)

# Displaying Pie Chart
plt.show()
```


    
![png](Youtube%20Comment%20Sentiment%20Analysis_files/Youtube%20Comment%20Sentiment%20Analysis_27_0.png)
    



```python
colors = ['#8fd175', '#f76c6c', '#ffd700']  # Custom colors
explode = (0.1, 0, 0)  # Highlight the Positive slice
plt.figure(figsize=(6, 6))
plt.pie(comment_counts, labels=labels, autopct='%1.1f%%', startangle=90, explode=explode,
        colors=colors, textprops={'fontsize': 12}, wedgeprops={'edgecolor': 'black'})
plt.title('Sentiment Distribution', fontsize=16, fontweight='bold')
plt.show()

```


    
![png](Youtube%20Comment%20Sentiment%20Analysis_files/Youtube%20Comment%20Sentiment%20Analysis_28_0.png)
    


### 

### Word Cloud 


```python
from wordcloud import WordCloud

text = ' '.join(relevant_comments)  # Combine all comments
wordcloud = WordCloud(width=800, height=400, background_color='white', colormap='viridis').generate(text)

plt.figure(figsize=(12, 5))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.title('Word Cloud of YouTube Comments', fontsize=14, fontweight='bold')
plt.show()

```


    
![png](Youtube%20Comment%20Sentiment%20Analysis_files/Youtube%20Comment%20Sentiment%20Analysis_31_0.png)
    


### Donut Chart 


```python
plt.figure(figsize=(8, 8))
plt.pie(comment_counts, labels=labels, autopct='%1.1f%%', startangle=90, colors=colors,
        wedgeprops={'edgecolor': 'black', 'width': 0.4})
plt.title('Sentiment Distribution (Donut Chart)', fontsize=16, fontweight='bold')
plt.show()

```


    
![png](Youtube%20Comment%20Sentiment%20Analysis_files/Youtube%20Comment%20Sentiment%20Analysis_33_0.png)
    


### Challenges Faced

- Filtering non-English comments accurately.

- Handling edge cases like mixed-language comments or incomplete sentences.

- Managing API rate limits while fetching large volumes of comments.

### Conclusion

This project demonstrates the use of data science techniques to extract valuable insights from unstructured data like YouTube comments. By automating the process of sentiment analysis, this tool can help creators and analysts improve content strategies, enhance audience engagement, and maintain a positive online community.

### Future Enhancements

- Incorporate advanced NLP techniques like BERT for more nuanced sentiment analysis.

- Extend the analysis to replies in comment threads.

- Support for multiple languages to capture a global audience.

- Build a dashboard for real-time sentiment tracking.



### References

- YouTube Data API Documentation

- VADER Sentiment Analysis

- LangDetect Documentation


```python

```


```python

```


```python

```
