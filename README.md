# AI Career Intelligence & Interview Preparation Platform

## 1. Project Overview

An AI-powered career and interview preparation platform that connects a
candidate's resume, job opportunities, job descriptions, skill gaps,
personalized recommendations, and interview preparation into one system.

The platform is designed as an end-to-end candidate preparation
assistant.

### Core idea

``` text
Resume
  ↓
Candidate Profile
  ↓
Job Discovery
  ↓
Resume ↔ Job Matching
  ↓
Skill Gap Analysis
  ↓
Company/Role-specific Question Bank
  ↓
Mock Interview
  ↓
Performance Feedback
  ↓
Personalized Job Recommendations
```

The current system already contains two major capabilities:

1.  AI-powered Resume-to-JD Gap Analyzer
2.  AI Mock Interview Platform

The capstone extends these capabilities with:

3.  Personalized job recommendations based on previous
    searches/interactions
4.  Job discovery through web scraping
5.  Company/JD-specific interview question generation
6.  Candidate skill-gap and preparation workflow

------------------------------------------------------------------------

# 2. Problem Statement

Students and job seekers commonly use different platforms for:

-   finding jobs
-   checking whether their resume matches a job
-   identifying missing skills
-   preparing interview questions
-   practicing mock interviews
-   tracking suitable opportunities

These activities are disconnected.

This project aims to create one integrated platform where a candidate
can:

-   upload a resume
-   understand their skills and experience
-   discover relevant jobs
-   compare their resume with a job description
-   identify missing skills
-   receive personalized job recommendations
-   generate interview questions from a company's JD
-   practice a mock interview
-   receive performance feedback
-   continuously improve their preparation based on job interests

------------------------------------------------------------------------

# 3. Project Objectives

## Primary objectives

-   Build an intelligent resume analysis system.
-   Extract skills, education, experience, projects, and other candidate
    information.
-   Analyze a job description.
-   Calculate an explainable job-fit estimate.
-   Identify matched, partial, and missing skills.
-   Discover relevant job listings.
-   Track previous searches and interactions.
-   Recommend jobs based on candidate profile and interests.
-   Generate role/company-specific interview questions.
-   Conduct interactive mock interviews.
-   Provide interview performance feedback.
-   Provide a unified dashboard for the candidate.

## Secondary objectives

-   Demonstrate practical NLP and semantic search.
-   Demonstrate vector database usage.
-   Demonstrate RAG-style retrieval.
-   Demonstrate REST API development.
-   Demonstrate full-stack development.
-   Demonstrate database design.
-   Demonstrate AI-assisted personalization.
-   Provide a scalable architecture for future extensions.

------------------------------------------------------------------------

# 4. Existing Modules

## 4.1 Resume-to-JD Gap Analyzer

The existing analyzer supports:

-   Resume upload/paste
-   PDF, DOCX, and TXT parsing
-   Job description upload/paste
-   Resume section extraction
-   JD requirement extraction
-   Skill extraction
-   Skill normalization
-   Sentence Transformer embeddings
-   ChromaDB vector storage
-   Semantic retrieval
-   Explainable weighted scoring
-   Skill-gap analysis
-   Resume improvement recommendations
-   Role-specific interview question generation

The current AI-generated job-fit estimate uses:

``` text
Skills              40%
Experience          25%
Responsibilities    20%
Education           10%
Semantic Similarity  5%
```

Similarity classifications:

``` text
>= 0.80     Strong match
0.65-0.79   Partial match
0.50-0.64   Weak match
< 0.50      Gap
```

Important terminology:

> The score is an AI-generated job-fit estimate. It should not be
> represented as an employer's actual ATS score.

------------------------------------------------------------------------

# 5. Existing Mock Interview Module

The current mock interview application provides:

-   Candidate profile collection
-   Resume upload
-   Job description upload
-   Resume/JD matching
-   Interactive interview questions
-   Browser text-to-speech
-   Browser speech recognition
-   Webcam-based analysis
-   Face detection
-   Smile detection
-   Eye detection
-   Answer collection
-   Interview scoring
-   Strengths and improvement areas
-   Final performance dashboard
-   Interview recording

Current interview metrics include:

-   Confidence
-   Smile/warmth
-   Eye-contact proxy
-   Communication
-   Grooming
-   Personality
-   Overall score

The current visual/behavioral scoring is heuristic and should be
described as a prototype rather than a professionally validated
behavioral assessment.

------------------------------------------------------------------------

# 6. New Feature 1 --- Personalized Job Recommendations

## Goal

Recommend jobs based on:

1.  Previous searches
2.  Viewed/analyzed jobs
3.  Resume skills
4.  Candidate profile
5.  Semantic similarity
6.  Skill overlap

This feature should be implemented before web scraping because it can
work with existing or mock job data.

------------------------------------------------------------------------

# 7. Recommendation Architecture

``` text
                 ┌─────────────────┐
                 │ Candidate Resume│
                 └────────┬────────┘
                          ↓
                   Resume Profile
                          │
                          │
                 ┌────────┴────────┐
                 ↓                 ↓
          Resume Skills       Search History
                 │                 │
                 └────────┬────────┘
                          ↓
                  Recommendation
                      Engine
                          ↓
                 ┌────────┴────────┐
                 ↓                 ↓
          Semantic Similarity   Skill Overlap
                 │                 │
                 └────────┬────────┘
                          ↓
                   Ranked Jobs
                          ↓
                 Recommendation UI
```

------------------------------------------------------------------------

# 8. Search History

Create a `search_history` table.

## Fields

``` text
id
user_id / session_id
query
timestamp
```

Example:

``` text
AI Engineer
ML Engineer
GenAI Engineer
Python Developer
FastAPI Developer
```

Every time a user performs a job search, save the query.

------------------------------------------------------------------------

# 9. Recommendation API

## Save search

``` http
POST /api/search-history
```

Example:

``` json
{
  "query": "AI Engineer"
}
```

## Get history

``` http
GET /api/search-history
```

Returns recent searches.

## Get recommendations

``` http
GET /api/jobs/recommendations
```

Example response:

``` json
{
  "recommendations": [
    {
      "job_id": 12,
      "title": "AI Engineer",
      "company": "ABC",
      "location": "Bangalore",
      "match_score": 87,
      "reason": "Matches your searches for AI Engineer and your Python and ML skills."
    }
  ]
}
```

------------------------------------------------------------------------

# 10. Recommendation Algorithm

Keep the first version simple and explainable.

Suggested weighting:

``` text
Resume/Profile semantic similarity   50%
Previous-search similarity           30%
Skill overlap                        20%
```

Final score:

``` text
recommendation_score =
    0.50 * resume_similarity +
    0.30 * search_similarity +
    0.20 * skill_overlap
```

Normalize to:

``` text
0 - 100
```

## Fallback behavior

### No search history

Use resume/profile similarity.

### No resume

Use search-history similarity.

### Neither exists

Show an empty state requesting the user to upload a resume or perform a
search.

------------------------------------------------------------------------

# 11. Reuse Existing AI Infrastructure

Do not create a separate embedding system.

Reuse:

-   Sentence Transformers
-   Existing embedding service
-   Existing ChromaDB setup
-   Existing job/resume parsing services
-   Existing FastAPI architecture

The recommendation feature should be an extension of the existing
system.

------------------------------------------------------------------------

# 12. Job Data Model

If a job table does not already exist, create:

``` text
jobs
----
id
title
company
location
description
url
skills
source
created_at
scraped_at
```

Example:

``` json
{
  "title": "AI Engineer",
  "company": "ABC",
  "location": "Bangalore",
  "description": "Build ML APIs and LLM applications...",
  "url": "...",
  "skills": [
    "Python",
    "FastAPI",
    "Machine Learning",
    "LLM"
  ],
  "source": "example_source"
}
```

------------------------------------------------------------------------

# 13. Job Recommendation UI

Add a section to the dashboard:

## Recommended Jobs For You

Each card should display:

``` text
AI Engineer
ABC Technologies
Bangalore

Match: 87%

Python • ML • FastAPI • LLM

Why recommended:
Matches your previous searches and resume skills.

[Analyze] [View Job]
```

Also show:

## Recent Searches

``` text
AI Engineer
ML Engineer
GenAI Engineer
Python Developer
```

Recommendations should refresh after a new search.

------------------------------------------------------------------------

# 14. New Feature 2 --- Company/JD-specific Question Bank

## Goal

Generate interview questions based specifically on the selected
company's job description.

Input:

``` text
Company JD
+
Candidate Resume
```

Output:

``` text
Technical Questions
Resume-specific Questions
Role-specific Questions
Behavioral Questions
Difficulty levels
Topics to study
Expected answer points
```

------------------------------------------------------------------------

# 15. Question Generation Pipeline

``` text
Company JD
    ↓
JD Parser
    ↓
Requirements + Responsibilities
    ↓
Skill Extraction
    ↓
Topic Extraction
    ↓
Question Generator
    ↓
Question Bank
```

Resume can be added as additional context:

``` text
JD + Resume
     ↓
Question Generator
     ↓
Company/Role-specific questions
+
Resume-specific questions
```

------------------------------------------------------------------------

# 16. Question Categories

## Technical

Example:

``` text
Python
FastAPI
SQL
Machine Learning
Docker
AWS
LLMs
```

## Resume-specific

If the resume says:

> Built an LLM-powered chatbot.

Generate:

``` text
Explain the architecture of your chatbot.

Why did you choose your LLM/API?

How did you handle hallucinations?

How did you evaluate the chatbot?

How would you reduce latency?
```

## Behavioral

Examples:

``` text
Tell me about yourself.

Why are you interested in this role?

Tell me about a difficult project.

Describe a technical challenge you faced.

Tell me about a project where you worked in a team.
```

------------------------------------------------------------------------

# 17. Question Model

``` text
interview_questions
-------------------
id
job_id
category
topic
question
difficulty
expected_points
created_at
```

Possible categories:

``` text
technical
resume
behavioral
situational
company
role_specific
```

Difficulty:

``` text
easy
medium
hard
```

------------------------------------------------------------------------

# 18. New Feature 3 --- Job Discovery / Web Scraping

## Goal

Automatically collect relevant job listings.

The scraper should collect:

-   Job title
-   Company
-   Location
-   Description
-   Skills
-   Experience
-   Salary if available
-   Job URL
-   Source
-   Scraped timestamp

------------------------------------------------------------------------

# 19. Job Scraper Architecture

Use an adapter architecture rather than coupling the entire application
to one website.

``` text
                 JobSource Interface
                        │
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
      Source A       Source B      Source C
      Adapter        Adapter       Adapter
          │             │             │
          └─────────────┼─────────────┘
                        ↓
                   Normalized Job
                        ↓
                    Job Database
                        ↓
                 JD/Skill Analysis
```

Each adapter should return the same schema.

Important:

-   Respect website Terms of Service.
-   Respect robots rules where applicable.
-   Use official APIs when available.
-   Use reasonable request rates.
-   Avoid aggressive scraping.
-   Store the original source URL.
-   Deduplicate jobs.

Do not make the entire application dependent on a single site's HTML
structure.

------------------------------------------------------------------------

# 20. Job Processing Pipeline

``` text
Job Source
    ↓
Scraper/API
    ↓
Raw Job
    ↓
Cleaning
    ↓
Duplicate Detection
    ↓
JD Parser
    ↓
Skill Extraction
    ↓
Embedding
    ↓
Vector Database
    ↓
Relational Database
```

------------------------------------------------------------------------

# 21. Complete Platform Architecture

``` text
                         USER
                           │
                           ▼
                 ┌───────────────────┐
                 │ React Frontend    │
                 │ TypeScript        │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │ FastAPI Backend   │
                 └─────────┬─────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
 Resume Service       Job Service       Interview Service
        │                  │                  │
        ▼                  ▼                  ▼
 Resume Parser        Job Parser        Question Engine
        │                  │                  │
        └──────────┬───────┴──────────────────┘
                   ▼
             AI/NLP Layer
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
  Embeddings      RAG         LLM
       │           │           │
       └───────────┼───────────┘
                   ▼
               ChromaDB

                   +

              SQL Database
                   │
                   ▼
          Recommendation Engine
                   │
                   ▼
             User Dashboard
```

------------------------------------------------------------------------

# 22. Suggested Technology Stack

## Frontend

-   React
-   TypeScript
-   Vite
-   Tailwind CSS
-   lucide-react

## Backend

-   Python
-   FastAPI
-   Pydantic
-   Uvicorn

## Document Processing

-   PyMuPDF
-   python-docx

## AI/NLP

-   Sentence Transformers
-   scikit-learn
-   Embeddings
-   Semantic similarity
-   RAG
-   LLM abstraction

## Vector Database

-   ChromaDB

## Relational Database

Recommended:

-   PostgreSQL

Alternative for development:

-   MySQL
-   SQLite

## Computer Vision

-   OpenCV

## Speech

-   Browser Speech Recognition
-   Browser MediaRecorder
-   Text-to-Speech
-   Optional Whisper/backend transcription

## Deployment

-   Docker
-   Docker Compose

## Testing

-   pytest

------------------------------------------------------------------------

# 23. Database Architecture

Recommended relational entities:

``` text
users
  │
  ├── resumes
  │
  ├── search_history
  │
  ├── saved_jobs
  │
  ├── job_analysis
  │
  └── interview_sessions
          │
          └── interview_answers

jobs
  │
  ├── job_skills
  │
  └── interview_questions

skills
  │
  └── user_skills
```

------------------------------------------------------------------------

# 24. Suggested Database Tables

## users

``` text
id
name
email
created_at
```

## resumes

``` text
id
user_id
file_path
parsed_text
created_at
```

## skills

``` text
id
name
```

## user_skills

``` text
user_id
skill_id
proficiency
```

## jobs

``` text
id
title
company
location
description
url
source
created_at
scraped_at
```

## job_skills

``` text
job_id
skill_id
required
```

## search_history

``` text
id
user_id
query
timestamp
```

## saved_jobs

``` text
user_id
job_id
created_at
```

## job_analysis

``` text
id
user_id
job_id
fit_score
matched_skills
missing_skills
recommendations
created_at
```

## interview_questions

``` text
id
job_id
category
topic
question
difficulty
expected_points
```

## interview_sessions

``` text
id
user_id
job_id
overall_score
created_at
```

## interview_answers

``` text
id
session_id
question_id
answer
score
feedback
```

------------------------------------------------------------------------

# 25. API Structure

Suggested API groups:

``` text
/api/auth
/api/resume
/api/jd
/api/analysis
/api/jobs
/api/search-history
/api/recommendations
/api/interview
```

Example:

``` text
POST /api/resume/upload
POST /api/jd/upload
POST /api/jd/analyze

POST /api/analysis
GET  /api/analysis/{analysis_id}

GET  /api/jobs
GET  /api/jobs/{job_id}

POST /api/search-history
GET  /api/search-history

GET  /api/jobs/recommendations

POST /api/interview/questions
POST /api/interview/start
POST /api/interview/answer
POST /api/interview/stop
```

------------------------------------------------------------------------

# 26. RAG Architecture

``` text
Resume
   ↓
Parsing
   ↓
Cleaning
   ↓
Chunking
   ↓
Embedding
   ↓
ChromaDB
   ↓
Semantic Retrieval
   ↓
Relevant Context
   ↓
LLM
   ↓
Grounded Analysis
```

Use RAG when generating:

-   Resume improvement suggestions
-   JD gap explanations
-   Interview questions
-   Job-specific preparation
-   Candidate-specific explanations

The retrieved context should be used to reduce unsupported claims.

------------------------------------------------------------------------

# 27. Candidate Profile

The platform should maintain a normalized candidate profile:

``` text
Candidate
├── Education
├── Experience
├── Skills
├── Projects
├── Certifications
├── Preferred Roles
├── Search History
├── Viewed Jobs
├── Saved Jobs
└── Interview Performance
```

This profile becomes the basis for personalization.

------------------------------------------------------------------------

# 28. Personalized Recommendation Evolution

## Version 1

Content-based recommendation:

``` text
Resume + Search History → Job Similarity
```

## Version 2

Add:

``` text
Viewed Jobs
Saved Jobs
Analyzed Jobs
```

## Version 3

Add user interaction signals:

``` text
Viewed
Saved
Applied
Ignored
```

## Future

Collaborative filtering can be added if enough user data exists.

------------------------------------------------------------------------

# 29. Dashboard

The dashboard should eventually contain:

``` text
┌─────────────────────────────────────────────┐
│ Candidate Dashboard                         │
├─────────────────────────────────────────────┤
│                                             │
│ Resume Job Fit                              │
│                                             │
│ Recommended Jobs                            │
│                                             │
│ Recent Searches                             │
│                                             │
│ Skill Gaps                                  │
│                                             │
│ Interview Preparation                       │
│                                             │
│ Recent Interview Performance                │
│                                             │
│ Saved Jobs                                  │
│                                             │
└─────────────────────────────────────────────┘
```

------------------------------------------------------------------------

# 30. End-to-End User Flow

``` text
1. User registers/logs in
             ↓
2. Uploads resume
             ↓
3. Resume parser creates candidate profile
             ↓
4. User searches for jobs
             ↓
5. Search is stored
             ↓
6. Jobs are retrieved
             ↓
7. Recommendation engine ranks jobs
             ↓
8. User opens a job
             ↓
9. JD is analyzed against resume
             ↓
10. Skill gaps are displayed
             ↓
11. Interview question bank is generated
             ↓
12. User starts mock interview
             ↓
13. User answers questions
             ↓
14. System evaluates answers
             ↓
15. Feedback is generated
             ↓
16. User saves/views/interacts with jobs
             ↓
17. Recommendation profile becomes more personalized
```

------------------------------------------------------------------------

# 31. AI Components

## Resume intelligence

Tasks:

-   document parsing
-   section detection
-   skill extraction
-   skill normalization
-   experience extraction
-   education extraction

## Job intelligence

Tasks:

-   JD parsing
-   requirement extraction
-   skill extraction
-   responsibility extraction
-   experience requirement extraction
-   semantic embedding

## Matching

Tasks:

-   skill matching
-   semantic similarity
-   experience matching
-   responsibility matching
-   explainable score generation

## Recommendation

Tasks:

-   search-history similarity
-   resume similarity
-   skill overlap
-   job ranking

## Interview intelligence

Tasks:

-   question generation
-   answer transcription
-   semantic answer analysis
-   communication analysis
-   feedback generation

------------------------------------------------------------------------

# 32. Security

The system should:

-   validate uploaded file types
-   validate file sizes
-   never execute uploaded files
-   store secrets in environment variables
-   avoid exposing API keys
-   sanitize user input
-   validate API payloads with Pydantic
-   return user-friendly errors
-   avoid returning stack traces to users
-   protect user-specific data

------------------------------------------------------------------------

# 33. Testing Strategy

## Unit tests

Test:

-   resume parser
-   JD parser
-   skill normalization
-   similarity calculation
-   recommendation scoring
-   search-history service
-   question generation
-   API validation

## Integration tests

Test:

``` text
Resume upload
    ↓
Parsing
    ↓
Analysis
    ↓
Recommendation
```

and:

``` text
Job
 ↓
JD analysis
 ↓
Question generation
 ↓
Mock interview
```

## Recommendation tests

Test:

1.  Matching search
2.  Unrelated search
3.  Empty search history
4.  Resume-only fallback
5.  Duplicate jobs
6.  Score normalization

------------------------------------------------------------------------

# 34. Known Limitations

Current/expected limitations include:

-   Resume parsing can fail on unusual document layouts.
-   Skill knowledge base may not cover every technology.
-   Semantic similarity is an approximation.
-   Recommendation quality depends on available job data.
-   Search-history personalization requires sufficient user interaction.
-   Web scraping can break when source websites change their structure.
-   Scraping must respect each site's terms and technical restrictions.
-   Mock interview behavioral metrics are heuristic.
-   Browser speech recognition depends on browser and microphone
    support.
-   Local vector storage is not automatically suitable for large
    multi-user production deployment.
-   LLM-generated content requires grounding and validation.

------------------------------------------------------------------------

# 35. Future Enhancements

Possible future additions:

-   Authentication and OAuth
-   Persistent user profiles
-   Job application tracking
-   Resume version management
-   Multiple resumes for different roles
-   Recruiter dashboard
-   Application status tracking
-   Email notifications
-   Interview scheduling
-   Calendar integration
-   Learning roadmap generation
-   Course/resource recommendations
-   Advanced recommendation models
-   Collaborative filtering
-   Feedback-based recommendation learning
-   Whisper-based transcription
-   Better answer evaluation
-   Exportable interview reports
-   PDF career reports
-   Cloud deployment
-   Background workers
-   WebSocket progress updates
-   Production vector database
-   Analytics dashboard

------------------------------------------------------------------------

# 36. Recommended Development Order

Because the project already has two major components, implement the
remaining features in this order:

``` text
PHASE 1
✅ Resume/JD Gap Analyzer
✅ Mock Interview

PHASE 2
🔥 Personalized Job Recommendations
   ├── Search history
   ├── Job model
   ├── Recommendation service
   └── Recommendation UI

PHASE 3
🔥 JD/Company-specific Question Bank
   ├── JD skill extraction
   ├── Question generation
   ├── Difficulty
   └── Resume-specific questions

PHASE 4
🔥 Job Discovery
   ├── Job source adapters
   ├── Scraping/API
   ├── Cleaning
   ├── Deduplication
   └── Database storage

PHASE 5
🔥 Integration
   ├── Unified dashboard
   ├── User profile
   ├── Saved jobs
   ├── Interview history
   └── Recommendation loop

PHASE 6
🔥 Deployment
   ├── Docker
   ├── PostgreSQL
   ├── Production server
   └── Cloud deployment
```

------------------------------------------------------------------------

# 37. Capstone MVP

If development time becomes limited, the minimum viable capstone should
contain:

``` text
1. Resume upload
2. Resume parsing
3. JD analysis
4. Job-fit estimate
5. Skill-gap analysis
6. Job database
7. Search history
8. Personalized recommendations
9. JD-specific question generation
10. Mock interview
11. Interview feedback
12. Dashboard
```

Web scraping can be added after the core system works.

------------------------------------------------------------------------

# 38. Project Structure

Suggested final structure:

``` text
ai-career-platform/
│
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   └── routes/
│   │   │       ├── auth.py
│   │   │       ├── resume.py
│   │   │       ├── jd.py
│   │   │       ├── analysis.py
│   │   │       ├── jobs.py
│   │   │       ├── recommendations.py
│   │   │       ├── search_history.py
│   │   │       └── interview.py
│   │   │
│   │   ├── core/
│   │   ├── models/
│   │   ├── schemas/
│   │   ├── services/
│   │   │   ├── resume_service.py
│   │   │   ├── jd_service.py
│   │   │   ├── embedding_service.py
│   │   │   ├── retrieval_service.py
│   │   │   ├── recommendation_service.py
│   │   │   ├── job_service.py
│   │   │   ├── question_service.py
│   │   │   └── interview_service.py
│   │   │
│   │   ├── scrapers/
│   │   │   ├── base.py
│   │   │   ├── source_a.py
│   │   │   └── source_b.py
│   │   │
│   │   ├── prompts/
│   │   ├── utils/
│   │   └── main.py
│   │
│   ├── tests/
│   ├── requirements.txt
│   └── Dockerfile
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── hooks/
│   │   ├── types/
│   │   └── App.tsx
│   ├── package.json
│   └── Dockerfile
│
├── data/
│   └── skills.json
│
├── docker-compose.yml
├── .env.example
├── README.md
└── LICENSE
```

------------------------------------------------------------------------

# 39. Environment Variables

Example:

``` env
DATABASE_URL=

LLM_PROVIDER=disabled
OPENAI_API_KEY=
OPENAI_BASE_URL=
OPENAI_MODEL=

EMBEDDING_PROVIDER=sentence-transformers
EMBEDDING_MODEL=sentence-transformers/all-MiniLM-L6-v2

VECTOR_STORE_PROVIDER=chroma
CHROMA_PERSIST_DIR=./.chroma

VITE_API_BASE_URL=http://localhost:8000/api
```

------------------------------------------------------------------------

# 40. Running the Project

## Backend

``` bash
cd backend

python -m venv .venv

# Windows
.venv\Scripts\activate

pip install -r requirements.txt

uvicorn app.main:app --reload
```

## Frontend

``` bash
cd frontend

npm install

npm run dev
```

Typical development URLs:

``` text
Frontend:
http://localhost:5173

Backend:
http://localhost:8000

FastAPI Docs:
http://localhost:8000/docs
```

------------------------------------------------------------------------

# 41. Docker

``` bash
copy .env.example .env

docker compose up --build
```

------------------------------------------------------------------------

# 42. Project Evaluation Metrics

The project can be evaluated using:

## Resume/JD analysis

-   Skill extraction accuracy
-   Match classification
-   Semantic similarity
-   Gap detection

## Recommendations

-   Relevance of recommended jobs
-   Search-history relevance
-   Skill-match relevance
-   Duplicate rate

## Question generation

-   JD relevance
-   Resume relevance
-   Topic coverage
-   Difficulty distribution

## Interview

-   Answer completeness
-   Semantic relevance
-   Communication metrics
-   User feedback

------------------------------------------------------------------------

# 43. Academic Contribution

The project demonstrates integration of:

-   Natural Language Processing
-   Semantic embeddings
-   Vector databases
-   Retrieval-Augmented Generation
-   Recommendation systems
-   Document intelligence
-   Computer vision
-   Speech processing
-   Full-stack development
-   REST APIs
-   Database design
-   Software architecture
-   Docker-based deployment

Rather than demonstrating a single AI model, the capstone demonstrates
how multiple AI and software components can be integrated into one
practical application.

------------------------------------------------------------------------

# 44. Final Project Description

## Short version

> An AI-powered career intelligence and interview preparation platform
> that analyzes resumes and job descriptions, identifies skill gaps,
> discovers relevant job opportunities, recommends jobs based on
> candidate interests and previous searches, generates role-specific
> interview questions, and conducts AI-assisted mock interviews with
> personalized feedback.

## Detailed version

> The platform combines resume intelligence, semantic job matching,
> vector retrieval, RAG-based analysis, personalized job
> recommendations, job discovery, company/JD-specific interview question
> generation, and interactive mock interviews. A candidate can upload a
> resume, build a profile, discover relevant jobs, understand their
> job-fit and skill gaps, prepare for a specific role, practice through
> a mock interview, and receive performance feedback. The recommendation
> engine uses resume information, previous searches, and job similarity
> to personalize future opportunities.

------------------------------------------------------------------------

# 45. One-line Architecture Summary

``` text
Resume → Candidate Profile → Job Discovery → Job Matching → Skill Gap → Interview Preparation → Mock Interview → Feedback → Personalized Recommendations
```

------------------------------------------------------------------------

# 46. Final Vision

The final system should feel like a single career assistant rather than
a collection of unrelated features.

The user should be able to enter the platform once and move through:

``` text
UNDERSTAND ME
     ↓
FIND JOBS FOR ME
     ↓
TELL ME IF I MATCH
     ↓
TELL ME WHAT I'M MISSING
     ↓
PREPARE ME FOR THE ROLE
     ↓
INTERVIEW ME
     ↓
TELL ME HOW I DID
     ↓
FIND BETTER-MATCHED JOBS
```

This forms the complete capstone product:

# AI Career Intelligence & Interview Preparation Platform
