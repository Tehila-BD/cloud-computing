# 🌱 OnlyPlants - Smart Agricultural Monitoring & Gamification

Welcome to the **OnlyPlants** code repository!
Our app solves the challenge of remote agricultural monitoring and is designed for commercial farmers and advanced home growers. The system combines IoT sensors, powerful cloud-based artificial intelligence (AI) to identify pests and diseases, and a game-based leaderboard to encourage proper plant care.

## 🏗️ Explanation of the architecture
Our system relies on a distributed cloud architecture (Microservices / Serverless) to withstand loads and provide fast performance:
1. **IoT & Data Ingestion:** Humidity and temperature data are transmitted from the field in real time and are received by IoT services in the cloud. The architecture ensures that even without internet, the data will be stored locally and transmitted without loss of information.
2. **AI Processing:** Users upload images of the plants from their mobile. The images are saved in cloud storage, which triggers GPU-based functions that analyze the image to detect diseases and fungi in real time.
3. **Knowledge Base Search:** An internal search engine (implemented as a Microservice) allows for the slicing and ranking of agricultural guides for quick presentation to the user according to the disease detected.

## ⚙️ Server Documentation (Microservices Documentation)
As part of the system development, we created a smart search engine for plant guides built from 3 separate services:
* **`IndexService`:** Collects agricultural guides and generates an inverted index for quick search.
* **`QueryService`:** Receives the query from the user (e.g., searching for information about diseases). Supports logical operators (`AND`/`OR`) and contains a ranking mechanism that calculates a relevance score based on the frequency of appearance of the search terms in the guide.
* **`ResultService`:** The output service that receives the results from the QueryService, extracts a summary (Snippet) of the content, and formats the output for display on the mobile Dashboard.

## 💻 Usage Examples

This is how the system searches for relevant guides after the AI ​​has identified a problem with the plant. We will search for guides that contain the word `fungus` or `pests`:

```python
# Initialize the search engine services for OnlyPlants
index_service = IndexService()
query_service = QueryService(index_service)
result_service = ResultService(index_service, query_service)

# Enter guides into the database:
index_service.add_document({
'title': 'High Relevance Pests & Fungus Guide',
'content': 'Fungus is dangerous. Pests and fungus require fast treatment. Kill the fungus early.'
})
index_service.add_document({
'title': 'Medium Relevance Pests',
'content': 'How to protect your plants from pests effectively.'
})
index_service.add_document({
'title': 'Low Relevance Watering',
'content': 'Water your plants daily for optimal growth.'
})

# Perform the search (with ranking and OR operator)
query = query_service.create_query({'terms': ['fungus', 'pests'], 'operator': 'OR'})
formatted_results = result_service.format_results(query['id'])

=== Testing the ranking mechanism using the OR operator ===
Found 2 documents:
ID: 1 | Title: High Relevance Pests & Fungus Guide | Score: 4
ID: 2 | Title: Medium Relevance Pests | Score: 1
