# 🌱 OnlyPlants - Smart Agricultural Monitoring & Gamification

Welcome to the **OnlyPlants** code repository! This project documents our journey in Cloud Computing, evolving from basic IR search engines to advanced AI-driven systems.

## 📚 Course Curriculum & Lab Progress
This repository contains the implementation of the following labs:
1. **Introduction to Cloud Computing**
2. **JSON Data Handling**
3. **Firebase Integration**
4. **Data Visualization**
5. **Search Engine Implementation**
6. **IoT with ESP32**
7. **Microservices Architecture**
8. **RAG (Retrieval-Augmented Generation) System**

## 🏗️ System Architecture
Our system relies on a distributed cloud architecture (Microservices/Serverless) to withstand loads and provide fast performance:
1. **IoT & Data Ingestion:** Humidity and temperature data are transmitted from the field in real time. The architecture ensures data is stored locally (Edge) and transmitted without loss.
2. **AI Processing:** Users upload plant images. Cloud-based functions analyze these images to detect diseases and fungi in real time.
3. **Intelligent Knowledge Base (RAG Pipeline):**
   * **Retrieval:** Uses `SentenceTransformers` to convert queries and orchid research papers into semantic vectors.
   * **Vector Store:** Custom `SimpleVectorStore` ensuring high availability by utilizing in-memory computation.
   * **Generation:** Leverages LLMs to provide context-aware answers based on our academic literature.

## ⚙️ Server Documentation (Microservices Documentation)
We developed a smart search engine for agricultural guides composed of three services:
* **`IndexService`:** Collects agricultural guides and generates an inverted index.
* **`QueryService`:** Supports logical operators (`AND`/`OR`) and calculates relevance scores for ranking.
* **`ResultService`:** Extracts summaries (snippets) and formats the output for the mobile Dashboard.

## 📚 Academic Knowledge Base
Our system is powered by REAL academic literature concerning orchid cultivation and disease diagnosis:
1. *Control and Monitoring System of IOT-Based Orchid Cultivation* (Imansyah & Tunggadewi, 2023)
2. *Intelligent image analysis recognizes important orchid viral diseases* (Tsai et al., 2022)
3. *Current progress in orchid flowering/flower development research* (Wang et al., 2017)

## 🚀 Live Demo (Gradio)
Access the live interface here:
https://87bd32282a5803f612.gradio.live

## 💻 Usage Example

### Basic Search (Service-based):
```python
# Initialize search engine services
index_service = IndexService()
query_service = QueryService(index_service)
result_service = ResultService(index_service, query_service)

# Perform a search
query = query_service.create_query({'terms': ['fungus', 'pests'], 'operator': 'OR'})
formatted_results = result_service.format_results(query['id'])
