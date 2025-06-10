# Standard Operating Procedure (SOP) Template for OpenSAFELY Projects

## 🧭 Purpose
This SOP outlines a structured process for designing and executing a health data research project using the OpenSAFELY platform. It is intended to ensure consistency, reproducibility, and compliance with ethical and data governance standards.

## 👩‍⚕️ Intended Audience
- Researchers and analysts new to OpenSAFELY
- NHS data professionals contributing to research pipelines
- Project collaborators seeking to formalise workflow documentation

---

## 🔧 Prerequisites
Before initiating a project:
- ✅ Ethics and IG approvals confirmed (via sponsor or relevant authority)
- ✅ GitHub access to the project repository
- ✅ OpenSAFELY workspace permissions granted
- ✅ Familiarity with `study_definition.py`, cohort extraction, and `output-check`

---

## 📦 Project Setup

### 1. Define Study Purpose
Briefly state the research question, hypothesis, or policy objective.

### 2. Create GitHub Repository
- Use the [OpenSAFELY template repo](https://github.com/opensafely/cookiecutter-research)
- Include license, README, and documentation folder

### 3. Specify Study Variables
Edit `study_definition.py`:
- Define inclusion criteria
- Define exclusion criteria
- Specify required variables (age, sex, prescriptions, etc.)

---

## 🧪 Data Workflow

### 4. Build the Cohort
- Use `study_definition.py` and run:  
  ```bash
  opensafely generate-dataset analysis/study_definition.py --output output/input.csv.gz
