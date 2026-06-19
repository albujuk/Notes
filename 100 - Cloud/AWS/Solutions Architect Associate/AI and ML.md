---
domain: aws
track: solutions-architect-associate
topic: ai
type: note
tags:
  - aws
  - solutions-architect-associate
  - ai
  - machine-learning
  - rekognition
  - transcribe
  - polly
  - translate
  - lex
  - connect
  - comprehend
  - sagemaker
  - kendra
  - personalize
  - textract
---

# AI & ML Services

> For foundational concepts, see [[100 - Cloud/AWS/Cloud Practitioner/AI|Cloud Practitioner: AI]].

---

## Amazon Rekognition

Image and video analysis using ML. Find objects, people, text, scenes in images and videos using ML.

### Key Capabilities

- **Facial analysis and facial search**: user verification, people counting
- **Face database**: create a database of "familiar faces" or compare against celebrities
- **Content moderation**: detect inappropriate, unwanted, or offensive content (images and videos)
  - Used in social media, broadcast media, advertising, and e-commerce for safer user experience
  - Set a **Minimum Confidence Threshold** for items that will be flagged
  - Flag sensitive content for manual review in **Amazon Augmented AI (A2I)**
  - Help comply with regulations

### Use Cases

- Labeling
- Content Moderation
- Text Detection
- Face Detection and Analysis (gender, age range, emotions)
- Face Search and Verification
- Celebrity Recognition
- Pathing (e.g. sports game analysis)

---

## Amazon Transcribe

Automatically convert speech to text using deep learning-based **Automatic Speech Recognition (ASR)**.

### Key Features

- **PII redaction**: automatically remove Personally Identifiable Information
- **Automatic Language Identification**: for multi-lingual audio
- **Custom vocabulary**: improve transcription accuracy for domain-specific terms

### Use Cases

- Transcribe customer service calls
- Automate closed captioning and subtitling
- Generate metadata for media assets to create a fully searchable archive

---

## Amazon Polly

Text-to-Speech (TTS). Turn text into lifelike speech using deep learning.

### Lexicons & SSML

- **Pronunciation lexicons**: customize pronunciation of words
  - Stylized words: L337 => "Leet"
  - Acronyms: AWS => "Amazon Web Services"
  - Upload lexicons and use them in the SynthesizeSpeech operation
- **Speech Synthesis Markup Language (SSML)**: enables more customization
  - Emphasizing specific words or phrases
  - Phonetic pronunciation
  - Breathing sounds, whispering
  - Newscaster speaking style

---

## Amazon Translate

Natural and accurate language translation. Allows you to localize content (websites, applications) for international users, and translate large volumes of text efficiently.

---

## Amazon Lex & Connect

### Amazon Lex

Same technology that powers Alexa.

- **Automatic Speech Recognition (ASR)**: convert speech to text
- **Natural Language Understanding (NLU)**: recognize intent of text and callers
- Helps build chatbots, call center bots

### Amazon Connect

- Receive calls, create contact flows, cloud-based virtual contact center
- Can integrate with other CRM systems or AWS
- No upfront payments, 80% cheaper than traditional contact center solutions

---

## Amazon Comprehend

Fully managed, serverless **Natural Language Processing (NLP)** service. Uses ML to find insights and relationships in text.

### Key Capabilities

- Detect language of the text
- Extract key phrases, places, people, brands, or events
- Understand sentiment (positive/negative)
- Analyze text using tokenization and parts of speech
- Automatically organize a collection of text files by topic

### Use Cases

- Analyze customer interactions (emails) to find what leads to positive or negative experience
- Create and group articles by topics that Comprehend will uncover

### Amazon Comprehend Medical

Detects and returns useful information in unstructured clinical text:

- Physician's notes
- Discharge summaries
- Test results
- Case notes

Uses NLP to detect **Protected Health Information (PHI)** via the DetectPHI API.

**Data sources:** store documents in Amazon S3, analyze real-time data with Kinesis Data Firehose, or use Amazon Transcribe to transcribe patient narratives into text for analysis.

---

## Amazon SageMaker

Fully managed service for developers / data scientists to **build, train, tune, deploy, and apply ML models**.

> [!note] Covered at awareness level in [[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon SageMaker AI|Cloud Practitioner: SageMaker]]. Key SAA-depth details below.

### SAA-Depth Details

- **Built-in algorithms** vs custom models (bring your own framework)
- **Training jobs** vs **hosting endpoints** (real-time inference vs batch transform)
- **SageMaker Studio** (integrated IDE) vs **Notebook instances**
- **Model monitoring** and drift detection
- **AutoML (SageMaker Autopilot)**: automatic model selection and hyperparameter tuning
- **Feature Store**: store and share ML features for model building
- **Pipeline orchestration**: automate ML workflows
- **Cost considerations**: training (GPU instances, spot) vs inference (real-time endpoints, serverless inference)

---

## Amazon Kendra

Fully managed **document search service** powered by ML.

### Key Features

- Extract answers from within documents (text, PDF, HTML, PowerPoint, MS Word, FAQs)
- **Natural language search** capabilities
- **Incremental Learning**: learn from user interactions/feedback to promote preferred results
- Ability to **manually fine-tune** search results (importance of data, freshness, custom)

> [!tip] Exam Tip
> Kendra builds a knowledge base from your documents and provides natural language Q&A over them. It is not just keyword search; it understands context and returns precise answers.

---

## Amazon Personalize

Fully managed ML service to build applications with **real-time personalized recommendations**.

### Key Features

- Same technology used by Amazon.com
- Example: personalized product recommendations/re-ranking, customized direct marketing
- Example: user bought gardening tools, provide recommendations on the next one to buy
- **Integrates** into existing websites, applications, SMS, email marketing systems
- **Implement in days**, not months (no need to build, train, and deploy ML solutions yourself)

### Use Cases

- Retail stores, media and entertainment
- Personalized product recommendations
- Customized direct marketing

---

## Amazon Textract

Automatically extracts text, handwriting, and data from any scanned documents using AI and ML.

### Key Capabilities

- Extract data from **forms and tables**
- Read and process any type of document (PDFs, images)
- Goes beyond simple OCR: understands structure (key-value pairs, tables)

### Use Cases

- **Financial Services**: invoices, financial reports
- **Healthcare**: medical records, insurance claims
- **Public Sector**: tax forms, ID documents, passports

---

## Summary

| Service | What it does |
|---------|-------------|
| Rekognition | Face detection, labeling, celebrity recognition, content moderation |
| Transcribe | Speech to text (ASR), PII redaction, language identification |
| Polly | Text to speech (TTS), SSML, lexicons |
| Translate | Natural language translation |
| Lex | Build conversational bots (chatbots), NLU + ASR |
| Connect | Cloud contact center |
| Comprehend | NLP: sentiment, entities, key phrases, topic modeling |
| Comprehend Medical | NLP for clinical text, PHI detection |
| SageMaker | Build, train, deploy custom ML models |
| Kendra | ML-powered enterprise document search |
| Personalize | Real-time personalized recommendations |
| Textract | Extract text, handwriting, and data from documents (beyond OCR) |

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
