# LLM-Based Career Recommendation System

A hybrid system combining a Java Spring Boot web application (MBTI-based UI and user flows) with Python-based embedding and vector search components (Milvus) for career recommendations. This repository contains the web application under `Main/` and the embedding/search utilities under `WordEmbedding/`.

## Key Features
- MBTI-style questionnaire web UI served by a Spring Boot backend
- Career embeddings and vector search using Milvus
- Python embedding service and utilities for creating and searching embeddings
- Docker Compose for running a local Milvus + MinIO stack

## Repository Structure

- `Main/` — Spring Boot application (Maven) that serves the web UI and backend logic
  - `src/main/resources/templates/` — Thymeleaf templates and static HTML pages (index, questionnaire, results, etc.)
  - `src/main/resources/static/` — CSS and client-side assets
  - `pom.xml`, `mvnw` — Maven wrapper and build configuration
- `WordEmbedding/` — Python tooling for embeddings, Milvus integration, and recommender utilities
  - `embedding_service/` — embedding microservice code (e.g., `embedding_service.py`)
  - `milvusServer/` — docker-compose and scripts for running Milvus and loading embeddings
  - `career_embeddings_*.json` — saved embeddings and profiles
  - `requirements.txt` — Python dependencies for embedding/search utilities

## Prerequisites

- Java 11+ (or the version required by `Main/pom.xml`) and Maven (or use the included `mvnw` wrapper)
- Python 3.9+ (for the embedding/search utilities)
- Docker & Docker Compose (for running Milvus/MinIO locally)

Note: Check `Main/pom.xml` for the exact Java compatibility configured for the Spring Boot app.

## Quick Start

1. Run the Spring Boot web app (development)

```bash
cd Main
./mvnw spring-boot:run
# App will be available at http://localhost:8080/ by default
```

2. Build production JAR

```bash
cd Main
./mvnw package
# Run with: java -jar target/*.jar
```

3. Run the embedding service (Python)

```bash
cd WordEmbedding
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python embedding_service/embedding_service.py
# The service hosts endpoints for creating or retrieving embeddings (see the file for exact API)
```

4. Start Milvus + MinIO (local vector DB) using Docker Compose

```bash
cd WordEmbedding/milvusServer
docker-compose up -d
# Wait a few moments for services to be ready
```

5. Load embeddings into Milvus (one-time / data update)

```bash
cd WordEmbedding
# Example script that loads `career_embeddings_final.json` into Milvus
python milvusServer/loadingToMilvus.py
```

6. Search embeddings / run recommender

```bash
cd WordEmbedding
python searchMilvus.py
# Or use the web UI in `Main/` which integrates recommendations via backend endpoints
```

## Notes on Usage
- The Spring Boot app serves the UI pages found in `Main/src/main/resources/templates/`. These templates include the MBTI questionnaire, results, and recommendation pages.
- The Python embedding utilities provide tools to create embeddings (`create_embeddings.py`), run a small embedding service (`embedding_service/embedding_service.py`), and search/load embeddings with Milvus.
- `WordEmbedding/recommender.py` and related modules contain the logic for turning embedding matches into career recommendations and profiles.

## Development Tips
- When changing Java code, rebuild with `./mvnw package` or run `./mvnw spring-boot:run` for hot reload.
- For Python changes, re-create/refresh the virtual environment and re-install `requirements.txt`.
- If Milvus containers behave oddly, use `docker-compose down -v` in `WordEmbedding/milvusServer` to remove volumes and restart fresh (data may be lost).

## Tests
- Unit tests (if present) for the Spring Boot app can be run with:

```bash
cd Main
./mvnw test
```

## Contributing
- Fork the repo and open a pull request with a clear description of changes.
- For changes that affect embeddings or the vector index, include any scripts or instructions required to rebuild or load updated embeddings.

## License
This project does not include a license file by default. Add a `LICENSE` file to clarify terms for external contributors and users.

## Contact
If you want help running the system or integrating it with an external UI or API, open an issue or contact the repository owner.

---
_This README was generated to reflect the project layout found in the repository. If you'd like, I can update it with exact Java/Python version numbers (read from `pom.xml` and `requirements.txt`) and add example API endpoints._
