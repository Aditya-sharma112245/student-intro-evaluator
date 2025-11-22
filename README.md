# Student Introduction Evaluator

This is an AI-based tool that evaluates students’ self-introductions from transcripts and generates a rubric-based score along with detailed per-criterion feedback. The tool combines rule-based methods, NLP-based semantic scoring, and rubric-driven weighting.

---

## Features

- Evaluates transcripts of student self-introductions.
- Computes per-criterion scores based on:
  - Keyword presence
  - Flow & structure
  - Speech rate
  - Grammar
  - Vocabulary richness
  - Filler words
  - Sentiment
  - Semantic similarity
- Provides overall score (0–100) and detailed feedback in JSON format.
- Simple web UI using Streamlit.
- Optional deployment with ngrok for public access.

---

## Installation & Setup

1. Clone this repository and go to its folder:  
git clone <your-repo-url>  
cd <your-repo-folder>

2. Install dependencies:  
pip install -r requirements.txt

3. Download NLTK resources (run in Python):  
import nltk  
nltk.download('punkt')  
nltk.download('averaged_perceptron_tagger')

4. (Optional) Configure ngrok for public access:  
ngrok authtoken <your-ngrok-token>

5. Run the Streamlit app:  
streamlit run app.py

---

## Scoring Formula & Logic

- **Content & Structure** (max 40 points)  
  - Salutations (2–5 points)  
  - Must-have keywords like name, age, class, school, family, hobbies, goals, fun facts (up to 30 points)  
  - Flow of introduction (up to 5 points)

- **Speech Rate** (max 10 points)  
  - Optimal WPM: 111–140 → 10 points  
  - Slightly slow/fast → 6 points  
  - Too slow/too fast → 2 points

- **Language & Grammar** (max 10 points)  
  - Grammar mistakes counted using language_tool_python  
  - Normalized score based on errors per 100 words

- **Vocabulary** (max 10 points)  
  - Type-Token Ratio (TTR) of words used  
  - Higher TTR → higher score

- **Clarity / Filler Words** (max 15 points)  
  - Counts words like "um", "uh", "like", etc.  
  - Lower filler rate → higher score

- **Engagement / Sentiment** (max 15 points)  
  - Positive sentiment probability using VaderSentiment  
  - Higher positivity → higher score

- **Semantic Similarity** (max 10 points)  
  - Cosine similarity between transcript and rubric description using SentenceTransformer  
  - Higher similarity → higher score

- **Total Score** = sum of all above, normalized to max 100 points

---

## Feedback

The app produces a detailed JSON feedback including:  
- Per-criterion score (Salutation, Keywords, Flow, Speech Rate, Grammar, Vocabulary, Filler Words, Sentiment, Semantic Similarity)  
- Overall score  
- Number of grammar errors  
- Words count and filler word count  
- Keywords found  
- Semantic similarity value
