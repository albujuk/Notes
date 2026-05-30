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
  - sagemaker
  - pytorch
  - tensorflow
  - generative-ai
  - bedrock
  - amazon-q
---

# AWS AI / ML Services

AWS offers three tiers of AI/ML services, from fully managed pre-built models to raw infrastructure for custom solutions. For data pipeline and analytics services (Kinesis, Glue, Redshift, Athena, etc.) see [[Analytics]].

---

| Tier | What you get | Who it's for |
|------|-------------|--------------|
| 1: AI Services | Pre-built models, API access | No ML expertise needed |
| 2: ML Services | Build/train/deploy your own models (managed infra) | Data scientists |
| 3: ML Frameworks & Infra | Full control, your own frameworks on AWS compute | ML experts |

---

## Tier 1: AI Services

Pre-built, managed AI models. No ML expertise required; consume via API.

Three groups: **language**, **computer vision & search**, **conversational AI & personalization**.

---

## Language Services

Interpret text or speech and transform it into something meaningful.

### Amazon Comprehend

Natural language processing (NLP) to extract insights from documents: key phrases, language detection, sentiment, and common elements.

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

NLP-powered enterprise search. Understands query context to return precise answers, not just keyword-matched documents.

**Use cases:** Intelligent search, [[#Amazon Lex|chatbot]] backend search, application search integration.

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

## Tier 2: ML Services

More control over ML solutions without managing infrastructure. Build, train, and deploy custom models.

### Amazon SageMaker AI

Fully managed service to build, train, and deploy your own ML models. Provides an integrated IDE with simplified access control and transparency over ML projects. Features: experiment tracking, data visualization, workflow debugging/monitoring, and access to hundreds of pre-trained models deployable in a few steps.

**Key benefits:**

| Benefit | Description |
|---------|-------------|
| Choice of ML tools | Data scientists use the IDE; business analysts use the no-code interface |
| Fully managed infrastructure | SageMaker provides high-performance, cost-effective infra; focus on model development |
| Repeatable ML workflows | Automate and standardize MLOps practices with transparency and auditability |

---

## Tier 3: ML Frameworks and Infrastructure

Complete control over the ML training process. Use in-house expertise, ML frameworks, and AWS compute directly.

### ML Frameworks

Software libraries providing pre-built, optimized components for building ML models. AWS supports: **PyTorch**, **Apache MXNet**, **TensorFlow**.

### AWS ML Infrastructure

ML-optimized [[Compute#Amazon EC2|EC2]] instances, [[Analytics#Amazon EMR|Amazon EMR]], and [[Containers#Amazon ECS|Amazon ECS]] support custom solutions: high performance and flexibility for advanced ML workloads.

---

## Generative AI on AWS

Foundation model (FM) based services — higher-level than the 3-tier ML model, focused on deploying and adapting large pre-trained models.

### Amazon SageMaker JumpStart

ML hub with foundation models and pre-built ML solutions deployable in a few clicks. Extends [[#Amazon SageMaker AI]] with an FM catalog — browse, evaluate, and deploy models without writing training code.

**Use cases:** Rapid FM prototyping, fine-tuning FMs on custom data.

### Amazon Bedrock

Fully managed service for adapting and deploying foundation models from Amazon and leading AI companies (Anthropic, Cohere, Meta, Mistral, etc.). No infrastructure to manage — access FMs via API and customize them with your own data using techniques like RAG and fine-tuning.

**Use cases:** Generative AI applications, chatbots, content generation, document summarization.

### Amazon Q

Interactive AI assistant integrated with a company's information repositories (code, wikis, ticketing systems). Two variants: **Amazon Q Business** (enterprise knowledge assistant) and **Amazon Q Developer** (coding assistant, IDE integration). Also powers natural language queries in [[Analytics#Amazon QuickSight|Amazon QuickSight]].

**Use cases:** Internal knowledge search, code generation, developer productivity.

---

## Resources

| Service | Description |
|---------|-------------|
| [Amazon Comprehend](https://aws.amazon.com/comprehend/) | Use NLP to extract key insights from documents |
| [Amazon Polly](https://aws.amazon.com/polly/) | Convert text into lifelike speech |
| [Amazon Transcribe](https://aws.amazon.com/transcribe/) | Convert speech into text |
| [Amazon Translate](https://aws.amazon.com/translate/) | Translate text into multiple languages |
| [Amazon Kendra](https://aws.amazon.com/kendra/) | Intelligently query enterprise content with NLP |
| [Amazon Rekognition](https://aws.amazon.com/rekognition/) | Identify objects and activities in images and videos |
| [Amazon Textract](https://aws.amazon.com/textract/) | Detect and extract typed and handwritten text in documents |
| [Amazon Lex](https://aws.amazon.com/lex/) | Add voice and text conversational interfaces to applications |
| [Amazon Personalize](https://aws.amazon.com/personalize/) | Add personalized customer recommendations to applications |
| [Amazon SageMaker AI](https://aws.amazon.com/sagemaker-ai/) | Build, train, and deploy ML models without managing infrastructure |
| [Amazon SageMaker JumpStart](https://aws.amazon.com/sagemaker-ai/jumpstart/) | Deploy pre-trained ML solutions with a few clicks |
| [Amazon Bedrock](https://aws.amazon.com/bedrock/) | Fine-tune and integrate large FMs into AWS applications via single API |
| [Amazon Q Business](https://aws.amazon.com/q/business/) | Answer questions using company information repositories |
| [Amazon Q Developer](https://aws.amazon.com/q/developer/) | Accelerate development with code recommendations |

---

← [[100 - Cloud/AWS/Cloud Practitioner/README|Cloud Practitioner]]
