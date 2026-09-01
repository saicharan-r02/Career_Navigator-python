# 🧭 Career Navigator — AI Academic Analytics & Guidance SaaS

Career Navigator is an AI-powered academic analytics and career roadmap platform designed to help engineering students bridge the gap between university grades and professional career paths. By analyzing semester-wise performance through a Random Forest Classifier, the system predicts the most suitable career role, saves student progress in a persistent SQLite database, and generates an interactive, phase-wise roadmap for placement preparation.

---

## 🎯 Problem Statement

Engineering students often struggle to translate their academic performance into clear career direction and actionable preparation strategies. Existing guidance systems are generic and do not leverage individual academic data or save multi-semester preparation milestones. 

This platform solves this by:
1. Analyzing semester-wise performance across 50+ engineering subjects mapped to 9 core Technical Pillars.
2. Predicting tailored career roles (e.g. *Generative AI Engineer, Full Stack Web Engineer, DevOps & Platform Engineer, Data Scientist*).
3. Persisting student profiles, assessment history, and interactive phase-by-phase roadmap task checklists in a database.

---

## 🚀 Key Features

- 🎓 **Dynamic Grade Entry**: Interactive UI that adapts to different engineering branches (CSE, AI-ML, ECE, Mechanical, Civil) and academic years.
- 🧠 **AI-Driven Role Prediction**: Evaluates 50+ subjects and clusters them into 9 core "Technical Pillars" using a tuned Random Forest Classifier.
- 🗄️ **Persistent Database Layer (SQLite / SQLAlchemy)**: Automatically saves student profiles, past assessments, and customizable roadmap task checklists.
- 📊 **Skill Proficiency Dashboard**: Visualizes student strengths in Coding, Systems, Math, Hardware, Theory, Science, Design, Mechanical Core, and Civil Core.
- ✅ **Actionable Interactive Roadmaps**: Generates 6 structured preparation phases (Phases 0–5) with toggleable milestone progress tracking.
- 📄 **PDF Export**: Students can download their personalized career guide for offline use.
- 📈 **Institutional Analytics API**: Provides platform-wide trends on student career trajectories and milestone completion rates.

---

## 🛠️ Tech Stack

### Frontend:
* **React.js** (Functional Components & Hooks)
* **Axios** (REST API Communication)
* **jsPDF** (Automated PDF Document Generation)
* **Vanilla CSS3** (Modern Dark-Themed Glassmorphism UI)

### Backend & Machine Learning:
* **Python 3 & Flask** (REST API Server)
* **SQLite / SQLCipher & SQLAlchemy 2.0** (`career_navigator.db`)
* **Scikit-Learn** (Random Forest Classifier)
* **Pandas & NumPy** (Data Processing & Vector Aggregation)
* **Flask-CORS** (Cross-Origin Resource Sharing)

---

## 📁 Project Structure

```
Career_Navigator/
├── frontend/
│   ├── public/
│   │   └── index.html
│   ├── src/
│   │   ├── components/
│   │   │   └── GradeForm.js
│   │   ├── App.jsx
│   │   ├── App.css
│   │   ├── index.js
│   │   └── index.css
│   ├── package.json
│   └── package-lock.json
│
├── backend/
│   ├── server.js
│   ├── package.json
│   └── package-lock.json
│
├── ml_engine/
│   ├── database.py             # SQLAlchemy ORM models & student queries
│   ├── predict_logic.py        # Flask ML inference & roadmap API server
│   ├── career_navigator.db     # SQLite database (auto-generated on startup)
│   ├── model/
│   │   └── career_model.pkl    # Serialized ML model
│   └── requirements.txt        # Python dependencies (Flask, SQLAlchemy, Scikit-Learn)
│
├── .gitignore
└── README.md
```

---

## 🗃️ Database Schema & Inspection

The system stores persistent student information in [`career_navigator.db`](file:///c:/Users/saich/OneDrive/Desktop/python/Career_Navigator/ml_engine/career_navigator.db):

### Tables:
1. `students`: Stores student metadata (`email`, `full_name`, `branch`, `academic_year`).
2. `career_assessments`: Stores the predicted role, 9-pillar proficiency scores, and entered grades snapshot.
3. `roadmap_milestones`: Stores 6-phase preparation tasks with checklist completion flags (`is_completed`, `completed_at`).

### How to Inspect Stored Data:
Run this Python snippet in your terminal:
```bash
python -c "
import sqlite3, pandas as pd
conn = sqlite3.connect('ml_engine/career_navigator.db')
print('=== REGISTERED STUDENTS ===')
print(pd.read_sql_query('SELECT * FROM students;', conn))
print('\n=== CAREER ASSESSMENTS ===')
print(pd.read_sql_query('SELECT id, student_id, predicted_role, created_at FROM career_assessments;', conn))
print('\n=== ROADMAP TASKS CHECKLIST ===')
print(pd.read_sql_query('SELECT id, phase_index, phase_title, is_completed FROM roadmap_milestones LIMIT 6;', conn))
"
```
Or open `ml_engine/career_navigator.db` in **DB Browser for SQLite** or **VS Code SQLite Viewer**.

---

## 📡 REST API Documentation

### 1. `POST /predict` — Predict Career Role & Save Roadmap
* **Request Body**:
```json
{
  "email": "alex@university.edu",
  "full_name": "Alex Chen",
  "branch": "CSE",
  "academic_year": "3rd Year",
  "grades": {
    "Programming in C": "O",
    "Data Structures": "A+",
    "Operating Systems": "A"
  }
}
```
* **Response**:
```json
{
  "assessment_id": 1,
  "email": "alex@university.edu",
  "prediction": "Full Stack Web Engineer (MERN/Next.js)",
  "pillar_stats": {
    "coding": 95.0,
    "math": 80.0,
    "systems": 85.0
  },
  "roadmap": [
    "Phase 0: Web Fundamentals & Version Control...",
    "Phase 1: Advanced Frontend Mastery..."
  ]
}
```

### 2. `GET /api/student/history?email=alex@university.edu`
Fetches a student's past career evaluations and milestone task checklist statuses across sessions.

### 3. `POST /api/student/milestone/toggle`
* **Request Body**:
```json
{
  "milestone_id": 1,
  "is_completed": true
}
```

### 4. `GET /api/analytics`
Returns aggregate career trends across all students (most predicted roles, branch statistics, overall milestone completion rate).

---

## ⚙️ Installation & Setup

### 1. ML Engine & Backend Setup
```bash
cd ml_engine
pip install -r requirements.txt
python predict_logic.py
```

### 2. Frontend Setup
```bash
cd ../frontend
npm install
npm run dev
```

---

## 🖼️ Interface Screenshots

![Specialization and Semester Selection Interface](<Specialization and Semester Selection Interface-fig-1.png>)
![Core Subject Grade Entry Interface](<Core Subject Grade Entry Interface-fig-2.png>)
![Practical Laboratory Grade Entry Form ](<Practical Laboratory Grade Entry Form -fig-3.png>)
![Additional Laboratory and Workshop Entry](<Additional Laboratory and Workshop Entry-fig-4.png>)
![Final Transcript Submission and API Trigger ](<Final Transcript Submission and API Trigger -fig-5.png>)
![AI-Predicted Career Role and Skill Proficiency Dashboard](<AI-Predicted Career Role and Skill Proficiency Dashboard-fig-6.png>)
![Actionable Phase-Wise Career Guidance Roadmap (Phases 0-2)](<Actionable Phase-Wise Career Guidance Roadmap (Phases 0-2)-fig-7.png>)
![Advanced Roadmap Milestones and PDF Export Interface](<Advanced Roadmap Milestones and PDF Export Interface-fig-8.png>)

---

## 👨‍💻 Author

Developed by **Sai Charan** — AI & EdTech Systems Engineering.
