# USG-Assist-MP

USG Report Image Analysis System
AI-Assisted Ultrasound Report Extraction, Clinical Metric Parsing & Automated Medical Summary Generation
 Overview

This project is a Healthcare AI prototype built for Hack for Social Cause – VBYLD 2026.
It converts Ultrasound (USG) report images into:

Structured medical data

Auto-generated patient summaries

Auto-generated doctor interpretation

Downloadable PDF reports

Extracted clinical metrics

Basic risk scoring for early triage

This version uses Gemini (Google AI Studio) for OCR + clinical interpretation because it is faster to ship a prototype within hackathon timelines.

 Problem Motivation (Madhya Pradesh)

Rural & semi-urban districts in MP face:

Lack of radiologists → delayed USG interpretations

Heavily mixed Hindi–English reports → OCR complexity

Patients traveling 40–80 km for follow-ups

Poor record digitization → reports get lost

Antenatal cases (pregnancy) often miss critical abnormalities

This system provides an instant solution:
Take photo → AI extracts → AI interprets → PDF generated → Download/Share

This directly helps PHCs, CHCs, district hospitals, private clinics, and JANANI centers.

Read full justification:
 docs/state-justification-mp.md

 Architecture Summary

See full diagram:
 docs/architecture-diagram.md

High-level flow:

User Uploads USG Image
        │
        ▼
Gemini Vision OCR (Google AI Studio)
        │
        ▼
Medical NLP Extractor (Findings, Organs, Measurements)
        │
        ▼
Risk Engine (Pregnancy, Liver, Gallbladder, Kidney, Prostate)
        │
        ▼
Summary Generator (Patient Report + Doctor Note)
        │
        ▼
PDF Report Builder
        │
        ▼
Download / Share

 Sample Output

(Actual JSON file in sample_reports/output_sample.json)

{
  "patient_name": "Sita Bai",
  "age": 29,
  "gender": "Female",
  "exam_type": "USG Abdomen & Pelvis",
  "findings": {
     "liver": "Mild fatty changes",
     "gall_bladder": "Normal",
     "kidneys": "No calculi",
     "uterus": "Normal size",
     "ovaries": "No cysts"
  },
  "measurements": {
     "liver_size": "14.2 cm",
     "gb_wall": "2.5 mm",
     "rt_kidney": "9.8 cm",
     "lt_kidney": "9.7 cm"
  },
  "risk_score": "Low",
  "ai_doctor_summary": "Ultrasound shows mild fatty liver changes, no acute findings..."
}

 Demo

Your live prototype built using Google Gemini Studio (UI Workflow) lives here:

 prototype_ui/          google-studio-link.txt

 Future Roadmap

Located in:
  backend_plan/

Includes:

api_design.md

risk_engine_logic.md

future_code_skeleton.md

 License

MIT License — free for anyone to use, modify, improve.

 Team-
 Hemant verma
 Aayushi malviya
 Aditya verma
