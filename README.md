# Layout-Aware PDF Ingestion for Legal Contracts

## Overview

This project builds a **layout-aware PDF ingestion pipeline** for legal contracts (NDAs, MSAs, employment agreements).  
The goal is to convert PDFs into **structure-preserving chunks** that are reliable inputs for LLM-based systems.

Naïve PDF-to-text extraction often breaks clause boundaries, loses section hierarchy, and mixes headers/footers into content. This project focuses on **getting the structure right before using LLMs**.

---

## Scope

### Supported documents
- English legal contracts
- Publicly available PDFs
- Mostly digital PDFs, with OCR fallback for scanned pages

### Out of scope
- Legal reasoning or interpretation  
- Clause classification or risk analysis  
- Contract comparison or redlining  

This is an **infrastructure project**, not a legal AI.

---

## Key Features

- PDF text extraction with OCR fallback  
- Layout parsing using bounding boxes  
- Reading order reconstruction (multi-column aware)  
- Legal section detection (e.g. `1`, `1.2`, `1.2(a)`)  
- Section-level chunking optimized for LLMs  
- FastAPI service for document ingestion

---

## High-Level Pipeline

1. **PDF Ingestion**
   - Detect digital vs scanned PDFs
   - Route scanned pages through OCR

2. **Layout Parsing**
   - Extract text blocks with positional metadata
   - Normalize coordinates across pages

3. **Reading Order Reconstruction**
   - Detect columns
   - Sort text blocks top-to-bottom within columns

4. **Structure Detection**
   - Identify section headers using numbering patterns and layout cues
   - Filter headers and footers via repetition heuristics

5. **Chunking**
   - One chunk per legal section
   - Definitions and tables preserved intact
   - Token limits applied after structural boundaries

---

## Output Format

Each document is converted into structured chunks:

```json
{
  "section_id": "5.2(a)",
  "title": "Confidential Information",
  "content": "...",
  "page_range": [7, 8]
}
