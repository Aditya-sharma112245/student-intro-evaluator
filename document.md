# Student Introduction Evaluator - Deployment & Run Instructions

This document explains how to run the Student Introduction Evaluator app locally and optionally deploy it publicly using ngrok.

---

## Prerequisites

Before running the app, ensure you have:

- Python 3.8+ installed
- pip installed
- Git installed (if cloning repository)
- ngrok account (optional, for public deployment)

---

## Clone the Repository

Open a terminal or command prompt:

git clone <your-repo-url>  
cd <your-repo-folder>  

Replace `<your-repo-url>` with your GitHub repository URL.

---

## Install Dependencies

Install all required Python packages from `requirements.txt`:

pip install -r requirements.txt  

This will install:

- streamlit  
- language-tool-python  
- nltk  
- vaderSentiment  
- sentence-transformers  
- pyngrok  

---

## Download NLTK Resources

Open Python in terminal or create a small Python script:

import nltk  
nltk.download('punkt')  
nltk.download('averaged_perceptron_tagger')  

These resources are required for tokenization and NLP analysis.

---

## (Optional) Configure ngrok for Public Access

If you want to make your Streamlit app accessible over the internet:

1. Sign up on [ngrok](https://ngrok.com/) and get your auth token.
2. In terminal, run:

ngrok authtoken <your-ngrok-token>  

Replace `<your-ngrok-token>` with your actual token.

---

## Run the Streamlit App

In terminal:

streamlit run app.py  

- This will start the app locally at `http://localhost:8501/`.  
- If using ngrok, the public URL will be printed in the terminal.

---

## Using the App

1. Paste a student transcript into the text area.  
2. Enter the duration of the speech in seconds.  
3. Click **Score**.  
4. The app will display:  
   - Overall score (0–100)  
   - Detailed per-criterion feedback  
   - Keywords found  
   - Grammar and vocabulary statistics  
   - Semantic similarity value  

---

## Scoring Formula & Logic

The app calculates scores based on:

1. **Content & Structure** (max 40 points)  
   - Salutations: 2–5 points  
   - Must-have keywords (name, age, class, school, family, hobbies, goals, fun facts): up to 30 points  
   - Flow of introduction: up to 5 points  

2. **Speech Rate** (max 10 points)  
   - Optimal words per minute (WPM) 111–140 → 10 points  
   - Slightly fast/slow → 6 points  
   - Too slow/too fast → 2 points  

3. **Language & Grammar** (max 10 points)  
   - Grammar mistakes counted using `language_tool_python`  
   - Normalized score based on errors per 100 words  

4. **Vocabulary** (max 10 points)  
   - Type-Token Ratio (TTR) of words used  
   - Higher TTR → higher score  

5. **Clarity / Filler Words** (max 15 points)  
   - Counts words like "um", "uh", "like", etc.  
   - Lower filler rate → higher score  

6. **Engagement / Sentiment** (max 15 points)  
   - Positive sentiment probability using `VaderSentiment`  
   - Higher positivity → higher score  

7. **Semantic Similarity** (max 10 points)  
   - Cosine similarity between transcript and rubric description using `SentenceTransformer`  
   - Higher similarity → higher score  

8. **Total Score** = sum of all above, normalized to 100 points  

---

## Feedback

The app produces a detailed JSON feedback including:  
- Per-criterion score (Salutation, Keywords, Flow, Speech Rate, Grammar, Vocabulary, Filler Words, Sentiment, Semantic Similarity)  
- Overall score  
- Number of grammar errors  
- Words count and filler word count  
- Keywords found  
- Semantic similarity value  

---

## Notes

- The app is fully offline except for optional ngrok deployment.  
- Make sure `app.py`, `requirements.txt`, and `README.md` are in the same folder.  
- You can edit the `must_have_keywords` or `good_to_have_keywords` in `app.py` to customize the rubric.  

---

## Troubleshooting

- **NLTK errors:** Make sure you downloaded `punkt` and `averaged_perceptron_tagger`.  
- **Port in use:** Change Streamlit port with `streamlit run app.py --server.port <port_number>`.  
- **ngrok errors:** Check your internet connection and auth token.  

This completes the setup and deployment instructions for the Student Introduction Evaluator.
