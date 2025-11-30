# USG-Assist-MP

# Smart USG Report Analysis & Automated Medical Summary System

## Overview
This project extracts clinical information from ultrasound (USG) report images, identifies maternal and medical risks, and generates structured downloadable reports for patients and doctors.

It is designed to support early detection of high-risk conditions in regions like Madhya Pradesh where digital reporting and medical interpretation gaps delay treatment.

---

## Key Features
- **Image Upload Support**
- **OCR Extraction (Tesseract)**
- **Medical Field Extraction (Regex + NLP)**
- **Risk Classification for Key Conditions**
- **Clean UI for Patients & Doctors**
- **Downloadable PDF Report**
- **Works with Low-Quality Images**
- **Fast Local Processing (No cloud needed)**

---

## Tech Stack
- **Python** (Backend)
- **FastAPI**
- **Tesseract OCR**
- **React / HTML Frontend** (depending on your version)
- **ReportLab** (PDF generation)
- **Regex-based Medical NLP**

---

## Features Extracted Automatically
- Gestational age  
- AFI  
- Placenta position  
- Fetal biometry (BPD, HC, AC, FL)  
- Presentation (cephalic/breech)  
- Fatty liver  
- Kidney stones  
- Gallstones  
- Ovarian cysts  
- Fibroids  
- Cervical measurements  
- Doppler keywords  

---

## Risk Detection (Rule-Based Engine)
Flags:
- **High Risk:** Low AFI, placenta previa, breech late pregnancy, severe IUGR indicators  
- **Moderate Risk:** Borderline AFI, mild cysts, small fibroids  
- **Normal:** Normal measurements, position, and findings  

---

## How to Run Backend

pip install -r requirements.txt
uvicorn main:app --reload

yaml
Copy code

---

## How to Run Frontend (If React Version)

npm install
npm run dev

yaml
Copy code

---

## Project Structure

backend/
frontend/
docs/
sample_reports/
README.md
LICENSE
.gitignore

yaml
Copy code

---

## Sample Output (JSON)

{
"gestational_age": "34 weeks 2 days",
"afi": "7 cm",
"placenta": "Posterior",
"presentation": "Cephalic",
"risks": {
"high": ["Borderline AFI"],
"moderate": [],
"normal": ["Placenta normal", "Presentation normal"]
}
}

yaml
Copy code

---

## State Relevance (Madhya Pradesh)
- High maternal mortality  
- Late diagnosis due to paper reports  
- Rural–urban medical gap  
- Low specialist availability  

This system digitizes ultrasound findings instantly and supports frontline workers.

---

## Disclaimer
This tool is *not a medical device*. It assists clinicians; it does not replace diagnosis.

