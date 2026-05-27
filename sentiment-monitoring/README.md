# Speech Sentiment Monitoring (h_speech)

A real-time sentiment analysis tool that monitors speech or text input and evaluates emotional sentiment using an ensemble of machine learning and deep learning models. It features both a command-line listening script and an interactive Streamlit dashboard for real-time visualization of sentiment trends.

## Features

- **Real-Time Speech Recognition**: Uses `speech_recognition` to capture audio from the microphone and convert it to text in real-time.
- **Ensemble Sentiment Analysis**: Predicts sentiment (Positive/Negative) by combining predictions from four different models:
  - Naive Bayes
  - Support Vector Machine (SVM)
  - Deep Learning (Keras/TensorFlow)
  - RoBERTa (Hugging Face Transformers)
- **Sentiment Temperature**: Calculates an average "temperature" across models, scaled from -1 (Negative) to 1 (Positive), tracking conversation dynamics over time.
- **Streamlit Dashboard**: Provides a web interface to input text manually, visualize sentiment probability and temperature over time, and highlight emotional anomalies.
- **Data Logging**: Automatically logs all captured sentences, predictions, probabilities, and timestamps to a CSV file for further analysis.

## Project Structure

- `listen_predict.py`: Command-line tool that listens to microphone input, predicts sentiment, and logs data to a CSV.
- `rt_monitoring.py`: Streamlit application providing a real-time dashboard for sentiment tracking and analysis.
- `train_model.py` / `PRML_Assignment3.py`: Scripts used for preprocessing data and training the Naive Bayes, SVM, and custom Deep Learning models.
- `sentiment_dashboard.py` / `sentiment_dashboardv2.py` / `sentiment_dashboardv3.py`: Various iterations of the Streamlit dashboard for sentiment visualization.

## Prerequisites

Ensure you have Python 3.8+ installed. It is recommended to use a virtual environment.

You need to have the following libraries installed:
```bash
pip install pandas numpy scikit-learn matplotlib keras tensorflow transformers streamlit speechrecognition nltk
```

### NLTK Data
The project requires several NLTK datasets. You can download them by running:
```python
import nltk
nltk.download('stopwords')
nltk.download('omw-1.4')
```

### Models
The project relies on pre-trained models which should be located in the root directory:
- `naiveBayes_model.sav` (Naive Bayes model)
- `svm_model.sav` (SVM model)
- `adamax_5l_16ini.h5` (Deep Learning model)
- `roberta_sentiment_model/` (Fine-tuned RoBERTa directory)

## Usage

### 1. Speech Recognition Script

To run the microphone-based speech recognition and sentiment monitoring script:

```bash
python listen_predict.py
```
*Note: Make sure your microphone is connected and configured correctly.*

### 2. Streamlit Dashboard

To launch the real-time sentiment monitoring dashboard:

```bash
streamlit run rt_monitoring.py
```
This will open a local web server (usually at `http://localhost:8501`). Enter a target name to start capturing text input and visualizing their sentiment trend over time.

## Output Data

Both scripts will generate a CSV file named `<Target_Name>_sentiment_data.csv` in the root directory. This file contains detailed logs for each sentence, including individual model probabilities, average temperature, and timestamp.

## License
MIT License
