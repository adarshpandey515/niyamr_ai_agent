# Legal Act Analyzer

A small Jupyter notebook project to analyze a legal Act (PDF). The notebook performs three main tasks:

1. Generate a concise summary of the Act.
2. Extract structured section information (saved as JSON).
3. Run rule-based checks against the Act and output a pass/fail report (saved as JSON).

---

## Table of Contents

- [Requirements](#requirements)  
- [Files](#files)  
- [Usage](#usage)  
- [Outputs](#outputs)  
- [Repository Structure](#repository-structure)  
- [Features / Tasks](#features--tasks)  
  - [Task 1 — Act Summary](#task-1---act-summary)  
  - [Task 2 — Section Information Extraction](#task-2---section-information-extraction)  
  - [Task 3 — Rule-based Checks](#task-3---rule-based-checks)  
- [Example JSON Schemas](#example-json-schemas)  
- [Notes](#notes)

---

## Requirements

- Python 3.8+
- Jupyter Notebook
- Required Python packages (install with pip):

```bash
pip install PyPDF2 groq
```

(If you use a requirements file, add these packages to it.)

---

## Files

- `task.ipynb` — Main notebook that runs all tasks.  
- `<act_file>.pdf` — Any legal Act PDF you want to analyze (place in repo or point notebook to it).  
- `sections_output.json` — Generated structured section extraction.  
- `rules_output.json` — Generated rule check results.  
- `README.md` — This file.

---

## Usage

1. Put your Act PDF in the repository (or a location the notebook can access).  
2. Start Jupyter and open the notebook:

```bash
jupyter notebook task.ipynb
```

3. When prompted in the notebook, enter your Groq API key (or set it as an environment variable before launching):

On macOS / Linux:
```bash
export GROQ_API_KEY="your_api_key_here"
```

On Windows (PowerShell):
```powershell
$env:GROQ_API_KEY="your_api_key_here"
```

4. Run the notebook cells in order. The notebook will:
   - Read and clean the PDF text,
   - Call Groq models for summarisation and extraction,
   - Save outputs to JSON files.

---

## Outputs

- Printed summary (in the notebook).  
- `sections_output.json` — structured extraction of sections (definitions, obligations, eligibility, etc.).  
- `rules_output.json` — rule check results: an array of objects with pass/partial/fail, evidence, and confidence.

---

## Repository Structure (typical)

```text
.
├── task.ipynb
├── ukpga_20250022_en.pdf   # example Act (optional)
├── sections_output.json    # generated
├── rules_output.json       # generated
└── README.md
```

---

## Features / Tasks

### Task 1 — Act Summary
- Reads the Act PDF using PyPDF2.
- Cleans the extracted text (removing line breaks, extra spaces).
- Sends the cleaned text to a Groq model with a legal summarisation prompt.
- Produces a bullet-point summary based strictly on the provided Act text (no external knowledge).

### Task 2 — Section Information Extraction
- Sends the Act text to Groq with a strict extraction instruction.
- Asks the model to return strictly-formatted JSON capturing items such as:
  - definitions
  - obligations
  - responsibilities
  - eligibility
  - record_keeping
  - payments / entitlements
  - enforcement
  - reporting
- The notebook cleans the model output (removing fences), parses it with `json.loads`, and saves to `sections_output.json`.

### Task 3 — Rule-based Checks
- Compares the Act text and extracted sections against a list of drafting rules (e.g., “Act must define key terms”, “Act must specify eligibility criteria”).
- Sends instructions to Groq to return a JSON array of objects in the schema shown below.
- Cleans and validates the JSON, then writes results to `rules_output.json`.

---

## Example JSON Schemas

Example rule check output (schema):
```json
[
  {
    "rule": "Act defines key terms",
    "status": "pass",
    "evidence": "Definition of 'benefit' found in Section 2",
    "confidence": 0.92
  },
  {
    "rule": "Act specifies eligibility criteria",
    "status": "partial",
    "evidence": "Eligibility criteria present but not exhaustive",
    "confidence": 0.6
  }
]
```

Example sections extraction (schematic):
```json
{
  "definitions": {
    "benefit": "means ...",
    "person": "means ..."
  },
  "eligibility": {
    "criteria": [
      "resident of X",
      "over 18"
    ]
  },
  "obligations": {
    ...
  }
}
```

---

## Notes

- The notebook assumes Groq model usage; ensure you have a valid API key and understand any usage costs or rate limits.
- The system prompts used in the notebook are strict about JSON formatting — the notebook includes cleaning/parsing steps to ensure valid JSON outputs.
- The summaries and extractions are based entirely on the provided Act text; they do not incorporate external legal knowledge.

If you want, I can also:
- Add a CONTRIBUTING section or LICENSE,
- Create a small CLI script to run the notebook tasks headless,
- Or update the notebook cells to read API key from environment variables automatically.
