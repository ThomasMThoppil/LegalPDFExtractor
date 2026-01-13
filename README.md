Layout-Aware PDF Ingestion for Legal Contracts
Overview

This project builds a layout-aware PDF ingestion pipeline for legal contracts (NDAs, MSAs, employment agreements).
The goal is to convert PDFs into structure-preserving chunks that are reliable inputs for LLM-based systems.

Naïve PDF-to-text extraction often breaks clause boundaries, loses section hierarchy, and mixes headers/footers into content. This project focuses on getting the structure right before using LLMs.

Scope

Supported documents

English legal contracts

Publicly available PDFs

Mostly digital PDFs, with OCR fallback for scanned pages

Out of scope

Legal reasoning or interpretation

Clause classification or risk analysis

Contract comparison or redlining

This is an infrastructure project, not a legal AI.

Key Features

PDF text extraction with OCR fallback

Layout parsing using bounding boxes

Reading order reconstruction (multi-column aware)

Legal section detection (e.g. 1, 1.2, 1.2(a))

Section-level chunking optimized for LLMs

FastAPI service for document ingestion

High-Level Pipeline

PDF Ingestion

Detect digital vs scanned PDFs

Route scanned pages through OCR

Layout Parsing

Extract text blocks with positional metadata

Normalize coordinates across pages

Reading Order Reconstruction

Detect columns

Sort text blocks top-to-bottom within columns

Structure Detection

Identify section headers using numbering patterns and layout cues

Filter headers and footers via repetition heuristics

Chunking

One chunk per legal section

Definitions and tables preserved intact

Token limits applied after structural boundaries

Output Format

Each document is converted into structured chunks:

{
  "section_id": "5.2(a)",
  "title": "Confidential Information",
  "content": "...",
  "page_range": [7, 8]
}

Evaluation

The system is evaluated against a small gold set of 10 manually annotated contracts, focusing on:

Section boundary accuracy

Section numbering correctness

Reading order preservation

Clause continuity (no mid-section splits)

The emphasis is on correctness and reliability, not scale.

Tech Stack

Python

pdfplumber

Tesseract / PaddleOCR (OCR fallback)

FastAPI

Optional LLM usage for cleanup and normalization

Why This Project

Most failures in LLM-based legal systems come from poor document ingestion, not weak models.
This project demonstrates how layout-aware preprocessing dramatically improves downstream LLM reliability.

Status

🚧 Work in progress
Initial focus: NDAs and MSAs
