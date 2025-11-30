# Architecture Diagram

            ┌──────────────────────────┐
            │  User Uploads Image      │
            └───────────────┬──────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Gemini Vision OCR    │
                 └───────────────┬──────┘
                                 │
                                 ▼
                      ┌──────────────────────┐
                      │ Medical NLP Parser   │
                      └──────────────┬───────┘
                                     │
                                     ▼
                          ┌──────────────────────┐
                          │ Risk Engine          │
                          └───────────────┬──────┘
                                          │
                                          ▼
                             ┌────────────────────────┐
                             │ Summary Generator      │
                             └──────────────┬─────────┘
                                             │
                                             ▼
                                ┌──────────────────────┐
                                │ PDF Report Builder   │
                                └──────────────────────┘

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
