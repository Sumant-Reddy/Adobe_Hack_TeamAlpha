Adobe Hackathon 2025 — Round 1A: Intelligent PDF Outline Extractor
🚀 Challenge Overview
Theme: Connecting the Dots Through Docs

Your mission: Transform a raw PDF into a structured outline that machines can understand — including the document title and a hierarchical structure of headings (H1, H2, H3) with page numbers.

This forms the foundation for Round 1B, where intelligent linking and analysis will take place.

✅ What This Project Does
Given one or more PDF files in the /input directory, this system extracts:

Title (semantically inferred, multi-line supported)

Headings with levels (H1, H2, H3) and page numbers

Output JSON per file in the /output directory

Example Output:

json
Copy
Edit
{
  "title": "Understanding AI",
  "outline": [
    { "level": "H1", "text": "Introduction", "page": 1 },
    { "level": "H2", "text": "What is AI?", "page": 2 },
    { "level": "H3", "text": "History of AI", "page": 3 }
  ]
}
🧠 Approach
This solution combines:

📄 Low-level PDF parsing using pdfplumber to extract raw text with fonts, positions, sizes

📊 Font size histogram analysis to estimate structural hierarchy

💡 Semantic reasoning using all-MiniLM-L6-v2 from sentence-transformers to:

Infer meaningful titles

Classify section headers even when font clues are ambiguous

🛠️ Models & Libraries Used
Component	Details
sentence-transformers/all-MiniLM-L6-v2	Semantic similarity for headings & title
pdfplumber	Token-level PDF parsing
torch, transformers	Required by MiniLM
python:3.9-slim	Docker base image

⚠️ The model is saved and loaded locally (no internet calls, ≤ 200MB, CPU-only)

🐳 Dockerized Setup
✅ Build Image
bash
Copy
Edit
docker build --platform linux/amd64 -t mysolution:adobe1a .
✅ Run Extraction
bash
Copy
Edit
docker run --rm \
  -v $(pwd)/input:/app/input \
  -v $(pwd)/output:/app/output \
  --network none \
  mysolution:adobe1a
All .pdf files in /input are processed

Output .json files will appear in /output

⚙️ Project Structure
graphql
Copy
Edit
├── main.py                   # Entry point: reads PDFs, saves output
├── utils.py                  # PDF parsing, semantic classification, font logic
├── model/                    # Locally stored MiniLM model
├── input/                    # Put your PDFs here
├── output/                   # Extracted JSONs appear here
├── Dockerfile                # Hackathon-compliant container
└── README.md                 # You're reading it!
⚡ Constraints Complied
Constraint	Status
Model Size ≤ 200MB	✅ (~85MB MiniLM)
No Internet Access	✅ Offline-only
CPU-only (amd64)	✅
Execution Time ≤ 10 sec	✅ Tested on 50-page docs
Generalized Logic	✅ No filename-specific overrides
