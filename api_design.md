App Design — Smart USG Report Digitization & Maternal Risk Detection System
1. Purpose

This document describes the application-level design for the USG Report Image Analysis System: UI flow, components, data model, API contracts, UX behaviors, error handling, localization (Hindi+English), and deployment notes. It is written to support both the current Google Gemini Studio workflow prototype and a future full-stack implementation (FastAPI + React).

2. High-level user flows

Upload → Analyze → Download

User uploads a USG report image (camera photo or scan).

System runs OCR → text cleaning → field extraction → risk scoring → generates structured results.

User reviews structured output and can download:

Patient-friendly PDF

Doctor-focused PDF

Raw extracted JSON (for auditors)

History & Repeat Analysis (future)

Store patient records, show previous reports and trends (AFI/Growth curves).

Admin / PHC Dashboard (future)

View aggregated risk counts, export CSV, and manage keyword/rule dictionaries.

3. Target users & devices

Primary: ASHA / ANM workers, PHC doctors, clinic staff.

Secondary: Patients (view-only in patient language), district health officers.

Devices: Android smartphones (primary), tablets, desktop (for dashboard).

Constraints: intermittent connectivity, mixed Hindi-English inputs, low-end devices.

4. Screens / Components (Studio + future React mapping)
4.1 Screen: Landing / Home

Purpose: Quick explanation and primary CTA.

Elements:

Title + one-line problem statement

Upload button (camera & gallery)

Link to "How it works" modal

Small note: “Hindi + English supported”

4.2 Component: Upload Card

Inputs:

File picker (image/pdf)

Camera capture (preferred on mobile)

Behavior:

Show thumbnail preview

Simple image quality tips (good lighting, flat, straight)

Buttons: Analyze, Download PDF (disabled until analysis)

4.3 Screen: Processing / Progress

Purpose: Visual feedback during OCR & parsing

Elements:

Loader with text steps: “Uploading → OCR → Extracting → Scoring”

Cancel button (safely stops process)

4.4 Screen: Results / Structured Summary

Sections:

Patient Info (if detected)

Obstetric Data (GA, AFI, FHR, Presentation, Placenta)

Measurements (BPD, FL, HC, AC, EFW)

Gynecology & Abdominal Findings

Risk Summary (color-coded badges)

Full Extracted Text (collapsible)

Actions:

Download Patient PDF

Download Doctor PDF

Re-run / Edit extracted fields (manual correction)

Save to history (future)

4.5 Screen: Error / Manual Review

Shows raw OCR text and allows user to correct key fields.

Save corrections to improve future parsing (dictionary update).

4.6 Modal: Help / Guide

Short instructions for image capture and sample USG images.

5. Component details & UX rules
Badges & colors

Red: High Risk

Yellow: Moderate Risk

Green: Normal

Accessibility

Font sizes ≥ 16px for mobile body text.

Buttons reachable with one thumb.

Color contrast high; avoid color-only signals (also use icons/text).

Hindi labels: allow toggling primary language to Hindi.

Offline behavior (mobile)

If offline: queue upload locally and auto-sync when network returns.

Show queued status and timestamp.

Manual correction

Allow edit of core fields: GA, AFI, Placenta, Presentation, FHR.

If user edits, show “Edited by user” label in results and include correction in downloaded PDF.

6. Data model (structured JSON)
{
  "patient": {
    "name": null,
    "age": null,
    "gender": null,
    "id": null
  },
  "study": {
    "type": "USG Abdomen/Pelvis",
    "date": "2025-11-30"
  },
  "obstetric": {
    "gestational_age": "34 weeks 2 days",
    "afi": "7",
    "presentation": "cephalic",
    "placenta": "posterior",
    "fetal_heart_rate": "140"
  },
  "measurements": {
    "bpd": null,
    "fl": null,
    "hc": null,
    "ac": null,
    "efw": null
  },
  "findings": {
    "liver": "mild fatty change",
    "gallbladder": "no calculus",
    "kidney": "normal"
  },
  "risks": {
    "highRisk": ["Placenta previa"],
    "moderateRisk": ["Borderline AFI"],
    "normal": ["Fetal presentation normal"]
  },
  "raw_text": "Full OCR text here",
  "metadata": {
    "source": "studio",
    "lang": ["eng","hin"],
    "processing_time_ms": 1234
  }
}

7. API Contracts (Studio endpoints + future FastAPI)
Client → Studio (current prototype)

No formal REST required for Studio workflow; user uploads directly to Studio interface. Provide public project link in repo.

Future REST APIs (FastAPI) — design:

POST /api/extract

Input: multipart/form-data { file }

Output: 200 { ocr_text, structured_fields, measurements, findings }

POST /api/analyze

Input: { structured_fields }

Output: 200 { risks: { highRisk[], moderateRisk[], normal[] }, risk_score }

POST /api/generate-report

Input: { structured_fields, risks, patient_info, language }

Output: PDF stream (application/pdf)

GET /api/health

Returns status

Sample /api/extract response

{
  "ocr_text": "...",
  "structured_fields": { ... },
  "measurements": { ... },
  "findings": { ... }
}

8. Parsing & extraction logic (summary)

Preprocess OCR text: normalize whitespace, remove page headers, fix common OCR confusions (e.g., '0'↔'O', 'l'↔'1').

Use deterministic regex + dictionary matches for:

GA patterns (e.g., \d+\s*weeks(\s*\d+\s*days)?)

AFI (afi[:\s]*\d+(\.\d+)?)

Placenta terms (placenta.*(anterior|posterior|low|previa))

Use fallback word-search for Hindi keywords (add mapping file).

Tag ambiguous fields and show UI correction prompt.

9. Risk scoring rules (summary)

AFI < 5 → +30 (High)

AFI 5–8 → +10 (Moderate)

Placenta previa / low-lying → +40

Breech in 3rd trimester → +35

IUGR keywords → +30

Fibroid/cyst presence → +5 each (unless size explicit >3cm → +15)

Thresholds:

Score ≥ 60 → High

30–59 → Moderate

<30 → Low/Normal

Show risk reasons verbatim in UI.

10. Localization & language support

Primary UI languages: English + Hindi (Devanagari).

OCR configured with eng+hin.

UI strings provided as key/value i18n JSON for future translation.

Patient summary should be available in both languages (Studio can generate both variants).

11. Security & privacy

Do NOT store PHI publicly. If storing locally for demo, encrypt or anonymize names.

For hackathon demo, use de-identified sample images.

Future production: HTTPS, authentication (OAuth2 / API keys), role-based access (ASHA vs Doctor), server-side logging with PII redaction.

12. Analytics & monitoring (optional)

Track:

number_of_uploads, avg_processing_time_ms, extraction_accuracy_estimate

user_corrections_rate (improves parser)

Send minimal telemetry (no PHI) for debugging.

13. Error handling & user messages

OCR failed → show friendly message: “Image unclear — try retaking photo in daylight” with example thumbnail.

Extraction failed (no fields) → show editable raw text + “Mark key fields manually”.

PDF generation error → allow retry and provide download of raw JSON.

14. Testing plan (quick)

Unit test regex extraction on 50+ sample USG texts (mix eng/hin).

Manual test flows on low-end Android (Samsung J2 / Redmi) to confirm UI responsiveness.

Edge-case tests: rotated images, low contrast, handwritten notes, multilingual token mixing.

15. Demo checklist (what to show in 3–4 min)

Upload sample image (good quality) → show OCR text briefly.

Show structured parsed fields (GA, AFI, Placenta).

Show risk badges (simulate a high-risk example).

Edit a field manually and save (demonstrates real-world flow).

Download patient PDF and open it.

Show repository docs and architecture diagram.

16. Deployment notes

Google Studio prototype: publish workflow and provide public link in repo.

Future backend: host FastAPI on Uvicorn + small VM (DigitalOcean / GCP) behind HTTPS; React site on Vercel/Netlify.

17. Maintainability & tuning

Maintain keyword_dictionary.json for Hindi/English terms; allow admins to extend keywords without redeploying code.

Keep risk rules declarative in risk_rules.json for easy tuning by clinicians.

18. Appendix — sample UI wireframe (text)

Header: USG-Assist MP | Upload

Upload Card (center)

After analyze: left column = Structured Summary; right column = Full text + Edit

Bottom: Download buttons + Save to history