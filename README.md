#Legal Act Analyzer
Simple notebook that:
- Reads a **PDF of a legal act**
- Generates a **summary**
- Extracts **structured section info (JSON)**
- Checks **rules in the Act** and returns a **pass/fail report**
- ### Files  task.ipynb # Main code sections_output.json # Extracted sections (generated) rules_output.json # Rule check results (generated) <act_file>.pdf # Any legal PDF you analyze

  ### Requirements ``` pip install PyPDF2 groq ``` --- ### How to Run 1. Put your **PDF** in the repo. 2. Open notebook: ``` jupyter notebook task.ipynb ``` 3. Enter your **Groq API key** when asked. 4. Run all cells in order. --- ### Output - **Summary** (printed in notebook) - **sections_output.json** - **rules_output.json** ---\


  
### Output
- **Summary** (printed in notebook)
- **sections_output.json**
- **rules_output.json**

Repo Structure

Typical structure (you can adjust to your actual files):

.
├── task.ipynb              # Main notebook with all 3 tasks
├── ukpga_20250022_en.pdf   # Example Act (Universal Credit Act 2025)
├── sections_output.json    # Structured section extraction (generated)
├── rules_output.json       # Rule check results (generated)
└── README.md               # This file


Features / Tasks
#Task 1 – Act Summary

Loads the Act PDF using PyPDF2.

Cleans the text (removes line breaks, extra spaces, etc.).

Sends the cleaned act text to a Groq model with a strict legal summarisation system prompt.

Produces a bullet-point summary of the Act based only on the provided text (no external knowledge).

#Task 2 – Section Information Extraction

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

#Task 3 – Rule-based Checks

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
