# sms_spam_classification
SMS Spam Classification using Support Vector Machines (SVM)

A robust, production-grade Natural Language Processing (NLP) pipeline for classifying SMS messages as either Ham (Legitimate) or Spam, powered by scikit-learn and Linear Support Vector Classification (LinearSVC).

📊 Dataset Details

Source: UCI SMS Spam Collection Dataset via Kaggle

Total Samples: 5,572 raw messages (reduced to 5,158 unique messages after deduplication).

Class Distribution:

Ham: ~87.6% (4,516 messages)

Spam: ~12.4% (642 messages)

Class Imbalance Handling: Addressed using stratified train/test splits, stratified 5-fold cross-validation, and class_weight='balanced' in the SVM classifier.

🛠️ Pipeline Architecture

The pipeline uses a hybrid ColumnTransformer feature extraction strategy alongside text preprocessing:

Text Preprocessing & Normalization:

Lowercasing and tokenization.

Explicit masking of URLs (urltoken), email addresses (emailtoken), currency symbols (currencytoken), and phone numbers/shortcodes (phonetoken).

Preservation of high-signal spam punctuation (!, $, %, ?).

Noun/verb lemmatization via NLTK's WordNetLemmatizer.

Hybrid Feature Engineering (ColumnTransformer):

Word TF-IDF Vectorizer: Captures unigrams, bigrams, and trigrams (with custom token patterns for spam symbols and English stopword filtering).

Character TF-IDF Vectorizer (char_wb): Captures sub-word character patterns (3 to 5 n-grams) to robustly detect intentional misspellings and obfuscation (e.g., fr33, wkly).

Structural Metadata Features: Scaled numerical metrics including character count, word count, digit density, exclamation/question counts, currency counts, and uppercase character ratios.

Classification & Calibration:

LinearSVC: Optimized with L2 regularization (C) and balanced class weighting.

CalibratedClassifierCV: Wraps the tuned SVM model to provide well-calibrated probabilistic confidence scores (.predict_proba()).

⚙️ Installation & Usage

Prerequisites

Ensure you have Python 3.8+ installed along with pip.

1. Clone the Repository

git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY


2. Install Dependencies

Run the install cell at the beginning of the notebook or via terminal:

pip install wordcloud nltk scikit-learn pandas numpy matplotlib seaborn


3. Run the Jupyter Notebook

Open the notebook in Google Colab (using the badge below) or locally via Jupyter:

jupyter notebook Spam_SMS_NEW.ipynb


(Make sure to upload spam.csv when prompted if running locally or via Colab file upload).

📈 Model Evaluation Metrics

The optimized pipeline achieves exceptional performance across held-out test data and cross-validation folds:

Test Accuracy: ~99.03%

Precision (Spam): ~96.83%

Recall (Spam): ~95.31%

F1 Score (Spam): ~96.06%

Mean 5-Fold Cross-Validation F1 Score: 0.9583 ± 0.0093

⚠️ Limitations & Real-World Challenges (Concept Drift)

While the model yields high evaluation metrics, it exhibits notable real-world vulnerabilities due to the nature of the training data:

Temporal Dataset Bias: The UCI SMS Spam Collection dataset was curated between 2002 and 2007—an era dominated by promotional and lottery-based text spam ("Win a free ringtone", "Claim £1000 cash").

The Rise of Modern Smishing (Phishing): Modern threat vectors have evolved into smishing (SMS phishing involving financial institutions, fake parcel tracking, and credential harvesting). Because terms like "suspicious activity", "temporarily locked", and "verify identity" do not appear in historical spam corpora, out-of-distribution phishing texts can occasionally bypass keyword-based text vectors.

Generalization Gap: This highlights a core machine learning lesson: high test-set accuracy does not guarantee bulletproof real-world performance against evolving semantic threat landscapes. Future work requires augmenting benchmark datasets with modern phishing text corpora or utilizing transformer-based language models (e.g., DistilBERT).

📜 License

This project is open-source and available under the MIT License.
