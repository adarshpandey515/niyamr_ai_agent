# niyamr_ai_agent
An ai agent to read specific doc and then summerize it , check if the doc follow some set of rules or not

Legal Act Analyzer (Universal Credit Act 2025 – Groq + PyPDF2)

This repo contains a small pipeline that reads a UK Act PDF (example: Universal Credit Act 2025), cleans the text, and then uses Groq’s LLMs to do three things:

Summarise the Act

Extract structured section information (definitions, obligations, eligibility, etc.)

Run rule-based checks on the Act and output a JSON list of results

Everything is implemented in a single Jupyter notebook: task.ipynb.

Features / Tasks
Task 1 – Act Summary

Loads the Act PDF using PyPDF2.

Cleans the text (removes line breaks, extra spaces, etc.).

Sends the cleaned act text to a Groq model with a strict legal summarisation system prompt.

Produces a bullet-point summary of the Act based only on the provided text (no external knowledge).

Task 2 – Section Information Extraction

Sends the full Act text to a second Groq call with a strict extraction system message.

Asks the model to return STRICT JSON capturing (for example):

definitions

obligations

responsibilities

eligibility

record_keeping

payments / entitlements

enforcement

reporting

The notebook:

Cleans model output (removes ```json fences etc.).

Parses it into a Python dict with json.loads.

Saves it as sections_output.json.

Task 3 – Rule-based Checks

Uses the original Act text and extracted sections to check a list of legal drafting rules (e.g. “Act must define key terms”, “Act must specify eligibility criteria”, etc.).

Sends instructions to Groq to return a JSON array of objects with this schema:

{
  "rule": "string",
  "status": "pass / fail / partial",
  "evidence": "text from the Act",
  "confidence": 0
}


Cleans the output again (removes code fences, forces valid JSON).

Writes final result to rules_output.json.

Repo Structure

Typical structure (you can adjust to your actual files):

.
├── task.ipynb              # Main notebook with all 3 tasks
├── ukpga_20250022_en.pdf   # Example Act (Universal Credit Act 2025)
├── sections_output.json    # Structured section extraction (generated)
├── rules_output.json       # Rule check results (generated)
└── README.md               # This file

Requirements

Python packages used in the notebook:

PyPDF2

groq

nbformat (only if you want to manipulate notebooks programmatically)

Standard library: os, json, re, etc.

You can install the main deps with:

pip install PyPDF2 groq

Groq API Setup

The notebook uses Groq’s Python client.

Get a Groq API key from your Groq account.

In the notebook, it asks for the key using getpass:

from getpass import getpass
from groq import Groq
import os

os.environ["GROQ_API_KEY"] = getpass("Enter your GROQ API key: ")
client = Groq(api_key=os.environ["GROQ_API_KEY"])


The key is not hardcoded in the notebook, so it’s safe to push the notebook to GitHub as long as you never print the key.

How to Run

Place the PDF in the repo root
Make sure the file name in the notebook matches your PDF path:

pdfpath = "ukpga_20250022_en.pdf"


Open the notebook

jupyter notebook task.ipynb


Run the cells in order

First cells: install packages, extract and clean PDF text.

Middle cells: summarisation and section extraction (writes sections_output.json).

Last cells: rule checking (writes rules_output.json).

Check outputs

summary_bullets printed in the notebook.

sections_output.json & rules_output.json saved in the repo.
