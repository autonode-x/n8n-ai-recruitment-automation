# n8n AI Recruitment Automation

## Overview

A production-ready AI Recruitment Automation built with **n8n**, **Groq LLM**, **Google Drive**, **Google Sheets**, **Google Calendar**, **Gmail**, and **Slack**.

The workflow automatically receives candidate applications, downloads resumes, extracts PDF content, performs AI-powered resume analysis, calculates ATS fit scores, generates interview questions, schedules interviews, sends emails, updates HR records, and notifies the hiring team.

---

# Features

* AI Resume Parsing
* ATS Candidate Scoring
* AI Skill Extraction
* Experience Detection
* Technology Extraction
* Resume Evaluation
* Interview Question Generation
* Google Drive Resume Download
* PDF Resume Extraction
* Google Calendar Interview Scheduling
* Gmail Notifications
* Google Sheets Candidate Database
* Slack HR Notifications
* Structured JSON Output
* Production Ready Workflow

---

# Workflow

Webhook

↓

Input Validation

↓

Download Resume from Google Drive

↓

Extract PDF Text

↓

AI Resume Parser

↓

ATS Candidate Scoring

↓

Decision Engine

├── Shortlisted
│
├── Generate Interview Questions
│
├── Schedule Google Calendar Event
│
├── Send Interview Email
│
├── Update Google Sheets
│
└── Send Slack Notification

OR

Rejected

├── Send Rejection Email

└── Update Google Sheets

---

# Tech Stack

* n8n
* Groq LLM
* Google Drive API
* Google Sheets API
* Google Calendar API
* Gmail API
* Slack API
* AI Agents
* Structured Output Parser

---

# API Request

```json
{
  "candidate_name":"John Doe",
  "candidate_email":"john@gmail.com",
  "position":"Python Developer",
  "resume_url":"GOOGLE_DRIVE_FILE_URL"
}
```

---

# API Response

```json
{
  "success": true,
  "candidate": "John Doe",
  "position": "Python Developer",
  "status": "Shortlist",
  "fit_score": 91
}
```

---

# AI Modules

## Resume Parser

Extracts

* Skills
* Experience
* Education
* Certifications
* Projects
* Technologies

---

## Candidate Scoring

Returns

* ATS Fit Score
* Recommendation
* Candidate Rank
* Strengths
* Weaknesses

---

## Interview Question Generator

Generates

* Beginner Questions
* Intermediate Questions
* Advanced Questions

Based on

* Position
* Skills
* Experience

---

# Integrations

* Webhook
* Google Drive
* Google Sheets
* Google Calendar
* Gmail
* Slack
* Groq AI

---

# Production Features

* Input Validation
* Structured JSON Responses
* Error Handling
* Modular AI Agents
* Retry Mechanism
* ATS Evaluation
* HR Notifications
* Calendar Automation
* Email Automation

---

```
10.n8n-ai-recruitment-automation
│
├── workflow.json
├── README.md
├── LICENSE
├── .gitignore
│
├── assets
│   ├── workflow.png
│   └── architecture.png
│
├── sample
│   ├── request.json
│   └── response.json

```

# Future Improvements

* OCR Resume Support
* DOCX Resume Support
* LinkedIn Profile Analysis
* GitHub Repository Analysis
* Multi-language Resume Parsing
* HR Dashboard
* Candidate Ranking Dashboard
* PostgreSQL Database
* Redis Queue
* Docker Deployment
* Kubernetes Deployment

---

# Use Cases

* HR Automation
* Recruitment Agencies
* Startups
* Enterprises
* AI Hiring Assistant
* Resume Screening
* ATS Automation

---

# Author

**Piyush Kumar**

AI Automation Engineer

Specialized in n8n, AI Agents, LangChain, LangGraph, RAG Systems, Workflow Automation, and Business Process Automation.

---

⭐ If you found this project useful, don't forget to star the repository.