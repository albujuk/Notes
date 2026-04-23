---
domain: aws
track: cloud-practitioner
topic: ai
type: note
tags:
  - aws
  - cloud-practitioner
  - ai
  - machine-learning
  - nlp
  - computer-vision
  - comprehend
  - polly
  - transcribe
  - translate
  - kendra
  - rekognition
  - textract
  - lex
  - personalize
---

# AWS AI Services

Pre-built, managed AI models trained for specific functions. No ML expertise required — consume via API.

Three groups: **language**, **computer vision & search**, **conversational AI & personalization**.

---

## Language Services

Interpret text or speech and transform it into something meaningful.

### Amazon Comprehend

Natural language processing (NLP) to extract insights from documents — key phrases, language detection, sentiment, and common elements.

**Use cases:** Content classification, customer sentiment analysis, compliance monitoring.

### Amazon Polly

Converts text → lifelike speech. Supports multiple languages, genders, and accents.

**Use cases:** Virtual assistants, e-learning, accessibility for visually impaired users.

### Amazon Transcribe

Converts speech → text. Features: speaker identification, custom vocabulary, real-time transcription.

**Use cases:** Customer call transcription, automated subtitling, media metadata generation.

### Amazon Translate

Real-time and batch text translation across multiple languages.

**Use cases:** Document translation, multi-language application integrations.

---

## Computer Vision and Search Services

Extract insights from documents, images, and video.

### Amazon Kendra

NLP-powered enterprise search. Understands query context to return precise answers — not just keyword-matched documents.

**Use cases:** Intelligent search, chatbots, application search integration.

### Amazon Rekognition

Video and image analysis. Identifies objects, people, text, scenes, and activities in content stored in [[S3]].

**Use cases:** Content moderation, identity verification, media analysis, home automation.

### Amazon Textract

Detects and extracts typed and handwritten text from documents, forms, and tables.

**Use cases:** Financial, healthcare, and government form processing.

---

## Conversational AI and Personalization Services

Text/voice interfaces and personalized recommendations.

### Amazon Lex

Add voice and text conversational interfaces to applications. Uses NLU (natural language understanding) + ASR (automatic speech recognition) for lifelike conversations. Powers Alexa under the hood.

**Use cases:** Virtual assistants, FAQ bots, natural language app search.

### Amazon Personalize

Uses historical data to generate personalized recommendations.

**Use cases:** Streaming recommendations, product recommendations, trending content.

---

← [[Index]] · [[Home]]