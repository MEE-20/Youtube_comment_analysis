# YouTube Comment Analysis Backend

This project provides a backend API for analyzing YouTube (and other platform) comments for spam, sentiment, toxicity, and hate speech using a multi-task DistilBERT-based deep learning model.

## Features
- **Spam Detection**: Binary classification (spam or not spam)
- **Sentiment Analysis**: Multi-class (Positive, Neutral, Negative, Irrelevant)
- **Toxicity Detection**: Multi-label (toxic, severe_toxic, obscene, threat, insult, identity_hate)
- **Hate Speech Detection**: Multi-class (normal, offensive, hatespeech)
- **Batch Prediction**: Analyze multiple comments in a single request
- **REST API**: Built with FastAPI for easy integration

## Project Structure
```
backend/
├── app.py                # Flask API (legacy/single comment)
├── main.py               # FastAPI application (batch and modern)
├── requirements.txt      # Python dependencies
├── models/               # Model weights and tokenizer files
│   ├── multi_task_distilbert_final.pth
│   └── tokenizer_final/
│       ├── vocab.txt
│       ├── special_tokens_map.json
│       └── tokenizer_config.json
├── src/
│   └── utils/
│       └── utils.py      # Text cleaning utilities
└── ...
```

## Setup Instructions

### 1. Clone the Repository
Clone this project and navigate to the `backend` directory.

### 2. Create and Activate a Virtual Environment (Recommended)
```
python -m venv venv
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Dependencies
```
pip install -r requirements.txt
```

### 4. Model and Tokenizer Files
Ensure the following files exist:
- `models/multi_task_distilbert_final.pth` (PyTorch model weights)
- `models/tokenizer_final/` (directory with `vocab.txt`, `tokenizer_config.json`, `special_tokens_map.json`)

### 5. Run the FastAPI Server
```
uvicorn main:app --reload
```
```
uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4

This command runs your FastAPI app so it is accessible from anywhere (not just your computer), on port 8000, and can handle multiple requests at once using 4 worker processes
```
The API will be available at [http://127.0.0.1:8000](http://127.0.0.1:8000)

## API Usage

### Root Endpoint
- `GET /`
  - Returns a welcome message.

### Batch Prediction Endpoint
- `POST /predict_batch`
  - **Request Body:**
    ```json
    {
      "comments": [
        "This video is amazing! I learned so much.",
        "You are so stupid, stop posting.",
        "Check out my channel for free giveaways!",
        "I don't agree with your opinion, but nice effort.",
        "What a waste of time, this is terrible.",
        "Visit http://spamwebsite.com for free stuff!",
        "You're a genius, keep up the good work!",
        "This is so offensive and rude.",
        "I love this content, please make more!",
        "Why are you spreading hate here?"
      ]
    }
    ```
  - **Response Example:**
    ```json
    [
      {
        "spam": 0,
        "sentiment": "Positive",
        "toxicity": {
          "toxic": 0,
          "severe_toxic": 0,
          "obscene": 0,
          "threat": 0,
          "insult": 0,
          "identity_hate": 0
        },
        "hate_speech": "normal"
      },
      ...
    ]
    ```

### Interactive API Docs
- Visit [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs) for Swagger UI to interact with the API.

## Notes
- For single comment prediction, see `app.py` (Flask version).
- Make sure your model and tokenizer files match the expected formats.
- The API does not return confidence scores, only predicted labels/classes.

## License
This project is for academic/research use. Please cite appropriately if used in publications.
