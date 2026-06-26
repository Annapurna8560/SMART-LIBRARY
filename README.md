# Smart Library Assistant

A prototype project for a college library assistant that uses computer vision, NLP, retrieval-augmented generation, and demand prediction.

## Academic Mapping

### (a) Capability mapping
1. **Reads scanned book-cover images to identify title/author**
   - AI area: **Computer Vision (CV)**
   - Justification: extracting visual text and patterns from image data uses CV techniques like OCR, image classification, and feature detection.

2. **Answers questions using the library's own documents/catalogue, not generic knowledge**
   - AI area: **Retrieval-Augmented Generation (RAG)**
   - Justification: RAG combines a retriever, a knowledge base, and a generator to answer queries using library-specific documents, ensuring responses are grounded in internal catalogue content.

3. **Learns from borrowing history to predict high-demand books**
   - AI area: **Machine Learning (ML)**
   - Justification: predicting demand from historical borrowing patterns is a supervised learning problem using statistical models on usage data.

4. **Improves as new documents are added**
   - AI area: **RAG / Knowledge Base Updating**
   - Justification: adding new documents updates the retriever index and knowledge store, enabling improved answers from fresh library content.

### (b) How CV, NLP, and RAG interact for a book-cover query
1. **CV stage**
   - The assistant receives a scanned book-cover image.
   - A computer vision module performs OCR and image analysis to detect text regions, extract title/author, and possibly classify cover layout.

2. **NLP stage**
   - Extracted text is normalized and converted into a query intent.
   - NLP processes the text to identify keywords, named entities, and the semantic intent of the user question.

3. **RAG stage**
   - The query is sent to a retriever that searches the library's indexed catalogue and document store.
   - The retriever returns relevant passages from the knowledge base.
   - A generator composes a response using the retrieved passages, ensuring the answer is based on library documents rather than generic knowledge.

### (c) Best ML paradigm for demand prediction
- **Best paradigm**: **Supervised learning** (regression or classification) because historical borrow records are labeled data that can predict future demand.
- **Useful features**:
  - Book metadata: title, author, subject, genre, publication year
  - Borrowing history: total borrow count, borrow frequency, seasonal demand
  - User behavior: number of unique borrowers, repeat borrower count
  - Availability: number of copies, current stock
  - Temporal context: month, semester, exam periods, holidays

### (d) Real-world challenge and solution
- **Challenge**: new library documents may be added in a variety of formats and inconsistent metadata.
- **Solution**: use a document ingestion pipeline with format conversion, metadata extraction, and automated indexing so the knowledge base is updated consistently.

## Project Structure

- `src/app.py` - orchestrates the assistant pipeline.
- `src/cv/book_cover_reader.py` - reads book cover images and extracts title/author.
- `src/rag/retriever.py` - retrieves relevant catalogue passages.
- `src/rag/knowledge_base.py` - stores and indexes library documents.
- `src/rag/generator.py` - generates grounded answers.
- `src/prediction/demand_predictor.py` - predicts high-demand books.

## Data layout

- `data/docs/*.json` - library knowledge documents used by the RAG knowledge base.
- `data/borrow_history.csv` - historical borrowing data for demand prediction.
- `data/example_cover.jpg` - scanned book-cover image used by the CV pipeline.

## Getting Started

1. Activate the virtual environment and install dependencies:

```powershell
Set-Location 'D:\smart library'
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

2. Run the CLI assistant:

```powershell
python src/app.py
```

3. Use the root launcher:

```powershell
python smart_library_assistant.py
```

4. Run the web frontend:

```powershell
python web_app.py
```

Then open the browser at:

```
http://127.0.0.1:5000/
```

### CLI options

```powershell
python src/app.py --image data/example_cover.jpg --query "What is this book about?"
```

### Notes

- The assistant reads a book cover image and extracts title/author.
- It answers questions using documents in `data/docs/`.
- It predicts demand using `data/borrow_history.csv`.
- The web app provides a user-facing interface for upload and query.
