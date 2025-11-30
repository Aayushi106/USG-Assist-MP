# Future Backend Code Skeleton (FastAPI)

from fastapi import FastAPI, UploadFile
from utils.ocr import extract_text
from utils.parser import parse_fields
from utils.risk import calculate_risk
from utils.pdf import generate_pdf

app = FastAPI()

@app.post("/extract")
async def extract(file: UploadFile):
    text = await extract_text(file)
    data = parse_fields(text)
    return data

@app.post("/risk")
async def risk(data: dict):
    score = calculate_risk(data)
    return score

@app.post("/report")
async def report(data: dict):
    pdf = generate_pdf(data)
    return StreamingResponse(pdf)
