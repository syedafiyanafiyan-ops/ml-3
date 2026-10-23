Task 3 — Resume / Candidate Screening System
Build an ML system to automatically screen and rank resumes based on a given job role.

Features
✅ Resume text cleaning & parsing (supports pasted text or uploaded .txt files)
✅ Skill extraction & matching against the job description
✅ Candidate ranking based on role fit (skill overlap + TF-IDF text similarity)
✅ Skill gap identification (required skills missing from each resume)
Tech Stack
Backend: Python, Flask, scikit-learn (TF-IDF + cosine similarity)
Frontend: HTML, CSS, vanilla JavaScript
Project Structure
task3_resume_screening/
├── backend/
│   ├── app.py                    # Flask API
│   ├── screening.py              # Skill extraction, scoring, ranking engine
│   ├── requirements.txt
│   └── sample_data/
│       ├── job_description.txt   # Sample "Data Analyst" job description
│       └── resumes/              # 4 sample candidate resumes (.txt)
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── script.js
└── README.md
Getting Started
1. Backend
cd backend
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py
The API runs at http://localhost:5002.

2. Frontend
Open frontend/index.html in your browser. Click Load Sample Job + Resumes to try it instantly, or paste your own job description and upload .txt resumes.

Update API_BASE in frontend/script.js if your backend runs elsewhere.

API Reference
Method	Endpoint	Description
GET	/api/health	Health check
GET	/api/sample-data	Returns the sample job description + 4 sample resumes
POST	/api/screen	Body: {"job_description": "...", "resumes": [{"name":..,"text":..}]}
POST	/api/screen/upload	multipart/form-data: job_description field + resumes file(s)
Example response (/api/screen)
{
  "required_skills": ["excel", "power bi", "python", "sql", "statistics", ...],
  "candidates": [
    {
      "rank": 1,
      "name": "Priya Sharma",
      "matched_skills": ["excel", "power bi", "python", "sql", ...],
      "missing_skills": ["azure"],
      "skill_overlap_pct": 88.9,
      "text_similarity_pct": 61.2,
      "final_score_pct": 79.2
    }
  ]
}
How It Works
Skill extraction: matches resume/job-description text against a curated, extendable skills taxonomy (SKILLS_TAXONOMY in screening.py) — synonyms included (e.g. "power bi" / "powerbi").
Required skills: every taxonomy skill mentioned in the job description becomes a "required" skill for that screening run.
Scoring: final_score = 0.65 × skill_overlap + 0.35 × TF-IDF cosine similarity — this blends exact skill matching with broader contextual similarity between the full resume and job description text.
Ranking: candidates are sorted by final_score descending.
Skill gaps: required skills not found anywhere in a resume are flagged so a recruiter can ask about them directly.
Possible Extensions
Swap the keyword-based skill extractor for spaCy's PhraseMatcher or a fine-tuned NER model for more robust skill detection.
Accept PDF/DOCX resumes directly (e.g. via pdfplumber / python-docx).
Add configurable scoring weights per role (e.g. weight "must-have" vs "nice-to-have" skills differently).
Persist screening runs to a database and add a recruiter dashboard.
License
MIT — free to use and modify for learning or portfolio purposes.
