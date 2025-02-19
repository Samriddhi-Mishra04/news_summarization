# news_summarization
# AI News Summarizer

Overview
The AI News Summarizer is a web app that fetches the latest news headlines and generates concise summaries using NLP models. Built using **Gradio, NewsAPI, and Hugging Face's Transformers, it allows users to quickly grasp key information from current affairs.

Features
- Fetch real-time news from various categories.
- AI-powered summarization using Facebook's BART model.
- User-friendly UI built with Gradio.
- Category selection (General, Business, Tech, Sports, etc.).

Tech Stack
- Python
- Gradio (for UI)
- Hugging Face Transformers (for text summarization)
- NewsAPI (for fetching news articles)

Installation & Setup
Install Dependencies
```bash
pip install requests transformers newsapi-python gradio
```

Get a NewsAPI Key
1. Sign up at [NewsAPI](https://newsapi.org/).
2. Generate an API key.
3. Replace `your_api_key` in `app.py`.

Run the App in Google Colab
Copy and paste the following code into a **Google Colab** notebook:
python
import requests
import gradio as gr
from transformers import pipeline

# Set up the summarization model
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")

# News API Configuration
API_KEY = "your_api_key"
NEWS_API_URL = "https://newsapi.org/v2/top-headlines"

def get_news(category, country="in"):
    params = {"country": country, "category": category, "apiKey": API_KEY, "pageSize": 5}
    response = requests.get(NEWS_API_URL, params=params)
    return response.json().get("articles", []) if response.status_code == 200 else []

def summarize_article(text, max_len=120):
    summary = summarizer(text, max_length=max_len, min_length=50, do_sample=False)
    return summary[0]["summary_text"]

def summarize_news(category):
    articles = get_news(category)
    results = []
    
    for article in articles:
        title, source, url, content = article["title"], article["source"]["name"], article["url"], article["content"]
        if content:
            summary = summarize_article(content)
            results.append(f" **{title}**\n Source: {source}\n [Read Full Article]({url})\n\n **Summary:** {summary}\n---")
    
    return "\n".join(results) if results else "No articles found. Try a different category."

gr.Interface(
    fn=summarize_news,
    inputs=gr.Dropdown(["general", "business", "entertainment", "health", "science", "sports", "technology"], label="Choose News Category"),
    outputs="markdown",
    title=" AI News Summarizer",
    description="Fetches and summarizes the latest news articles using AI."
).launch()


Run the App
- Click Run Cell in Colab.
- Open the Gradio public link to access the news summarizer.

Usage
1. Select a news category from the dropdown menu.
2. The app fetches top 5 headlines from NewsAPI.
3. AI generates concise summaries for each article.
4. Click on the full article link if you want to read more.

Future Enhancements
- Add multi-language support.
- Integrate speech-to-text for summarizing TV news.
- Support for RSS feeds (for more free news sources).

License
This project is open-source under the MIT License.


Built using Python, Gradio, and Hugging Face

