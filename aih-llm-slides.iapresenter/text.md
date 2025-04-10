# LLM Tutorial
	AI in Healthcare
	MSAI Spr '25
	Chelsea Ramos
---
# Overview
	- Healthcare Issue
	- Methods Employed
	- Setting Up
	- Medical Dataset Used
		- Load Data
		- Clean & Merge Data
	- Prompts Engineered
	- Evaluation of Results
	- Ideas for Improvement
---
# Healthcare Issue
	Predict whether a patient needs a prediabetes care plan based on demographics and clinical observations.
	- A prediabetes care plan can significantly reduce the risk of developing type 2 diabetes.
	- Accurate decisions can help save lives and reduce healthcare costs.
---
# Methods Employed
	1. I'll evaluate In-Context Learning methods:
	    - Zero-Shot Learning (instructions only, no examples)
	    - Chain-of-Thought (include step-by-step reasoning)
	    - Few-Shot Learning (include example)
	    - Tree of Thought (evaluate multiple reasoning paths)
	2. Then try Binary Classification w/ embeddings generated via LLM
---
# Setting Up
	Model: [SmolLM2](https://huggingface.co/HuggingFaceTB/SmolLM2-360M) from Hugging Face
/assets/Screenshot 2025-04-09 at 6.50.42 PM.png
size: contain
---
# Medical Dataset Used
	- Publicly available synthesized data (7 MB of data with 100 patients)
	    - Tables used (3):
	        - patients
	        - observations
	        - careplans
	1. Download ***Latest Version of Synthea:** 100 Sample Synthetic Patient Records, CSV: 7 MB* from [https://synthea.mitre.org/downloads](https://synthea.mitre.org/downloads)
	2. Save and unzip your downloaded ***synthea_sample_data_csv_latest.zip*** to a data path you can access via this notebook.
---
#### Load Data
/assets/Screenshot 2025-04-09 at 2.59.36 PM.png
size: contain
/assets/Screenshot 2025-04-09 at 3.00.58 PM.png
size: contain
---
#### Clean & Merge Data Pt. 1
/assets/Screenshot 2025-04-09 at 3.08.08 PM.png
size: contain
/assets/Screenshot 2025-04-09 at 3.09.16 PM.png
size: contain
---
#### Clean & Merge Data Pt. 2
/assets/Screenshot 2025-04-09 at 3.09.55 PM.png
size: contain
/assets/Screenshot 2025-04-09 at 3.10.45 PM.png
size: contain
---
# Prompts Engineered
---
	Patient #1 - Needs Prediabetes Care Plan

---
# Evaluation of Results

---
# Ideas for Improvement

---
# Thank You!