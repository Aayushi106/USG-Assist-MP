# API Design (Future Backend)

POST /api/extract
- Input: USG image (base64 or multipart)
- Output:
    - OCR text
    - structured_fields {}
    - measurements {}
    - abnormalities {}

POST /api/risk-score
- Input: structured_fields
- Output: risk_score, risk_reasons[]

POST /api/generate-report
- Input: structured data
- Output: PDF (binary stream)
