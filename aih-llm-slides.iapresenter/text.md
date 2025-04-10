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
	- Binary Classification
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
#### Patient #1 - Needs Prediabetes Care Plan
/assets/Screenshot 2025-04-09 at 7.04.49 PM.png
size: contain
---
#### Zero-Shot Learning
	- I developed a simple question based on Patient #1's data:
/assets/Screenshot 2025-04-09 at 7.06.04 PM.png
size: contain
	- Response:
	`No, the patient does not require a prediabetes care plan.`
		- This was incorrect.
---
#### Chain-of-Thought: Prompting
	- This time, I asked to "think first" and include reasoning for CoT:
	```
		prompt1 = f"""
	    Think first and explain your reasoning on whether a patient requires a prediabetes care plan or not. Then, give a final decision.
	    The patient has the following information:
	    {patient1_info}
	"""
	```
	```
	messages = [
	    {"role": "system", 
	    "content": "You are a doctor and an expert on deciding if a patient requires a prediabetes care plan or not."},
	    {"role": "user", 
	    "content": prompt1},
	]
	```
---
#### Chain-of-Thought: Response
		Based on the patient's information, I would recommend that they require a prediabetes care plan. The patient's high income, high expenses not covered, and high BMI indicate that they may be at risk for developing prediabetes. Prediabetes is a condition where the body's blood sugar levels are higher than normal, but not high enough to be classified as diabetes. This condition increases the risk of developing type 2 diabetes, heart disease, and other health problems.
	
	    The patient's high cholesterol levels, high triglycerides, and high blood pressure also suggest that they may be at risk for developing these conditions. A prediabetes care plan would focus on managing these conditions through lifestyle changes, such as diet and exercise, and potentially medication if necessary.
	
	    Additionally, the patient's age (34 years old) and gender (female) may also indicate that they are at higher risk for developing prediabetes. Prediabetes is more common in women and older adults, and it's estimated that up to 1 in 4 women may develop prediabetes by the age of 65.
	
	    Based on these factors, a prediabetes care plan would be recommended for this patient. This plan would aim to help them manage their cholesterol, triglycerides, and blood pressure, and to maintain a healthy weight through a combination of diet and exercise. It would also include regular check-ups to monitor their progress and adjust their plan as needed.
	- CoT helped to get to the correct conclusion!
	- However, a lot of statements were made that may not be true (hallucinations)
---
#### Patient #2 - Prediabetes Care Plan Not Needed
	
/assets/Screenshot 2025-04-09 at 7.15.35 PM.png
size: contain
	- Next, I developed a prompt for Patient #2:
	```
		patient2_info = """
		    They are married, white, nonhispanic, female, 33 years old, not high income, and have expenses covered,
		    Triglycerides 128.0 mg/dL, Hemoglobin A1c/Hemoglobin.total in Blood 6.2%, and Body mass index (BMI) [Ratio] 28.3 kg/m2.
		"""
		prompt2 = f"""
		    Think first and explain your reasoning on whether a patient requires a prediabetes care plan or not. Then, give a final decision.
		    The patient has the following information:
		    {patient2_info}
		"""
	```
---
#### Chain-of-Thought + Few-Shot Learning: Prompting
	- Continuing the CoT conversation w/ Patient #1 (`prompt1`), I provided an example `response1`:
	```
		response1 = """
		    1. Triglycerides 125.4 mg/dL is within the normal range (less than 150 mg/dL).
		    2. Hemoglobin A1c/Hemoglobin.total in Blood 6.0% is between 5.7% to 6.4%, which is considered prediabetes.
		    3. Body mass index (BMI) [Ratio] 30.1 kg/m2 is considered obese (BMI >= 30).
		    Based on the patient's information, I would recommend that they require a prediabetes care plan.
		"""
		messages = [
		    {"role": "system", 
		    "content": "You are a doctor and an expert on deciding if a patient requires a prediabetes care plan or not."},
		    {"role": "user", 
		    "content": prompt1},
		    {"role": "assistant", "content": response1},
		    {"role": "user", 
		    "content": prompt2},
		    ]
	```
---
#### Chain-of-Thought + Few-Shot Learning: Response
	    1. Triglycerides 128.0 mg/dL is within the normal range (less than 150 mg/dL).
	    2. Hemoglobin A1c/Hemoglobin.total in Blood 6.2% is between 5.4% to 6.6%, which is considered prediabetes.
	    3. Body mass index (BMI) [Ratio] 28.3 kg/m2 is considered normal (BMI >= 25).
	    Based on the patient's information, I would recommend that they require a prediabetes care plan.
	- This conclusion was incorrect, but Few-Shot Learning helped to improve the structure of the response greatly!
	- Good structure helps eliminate hallucinations
---
#### Tree of Thought: Prompting
	- Last, I asked to consider multiple reasoning paths for ToT:
	```
		# Prompt engineered w/ the help of ChatGPT 4o:
		prompt3 = f"""
		    Task: Decide if a patient needs a prediabetes care plan.
		
		    Step 1: Generate 3 possible reasoning paths.
		    Option A: ...
		    Option B: ...
		    Option C: ...
		
		    Step 2: Evaluate each option for correctness and relevance.
		    Evaluation: ...
		
		    Step 3: Select or combine the best option.
		    Final Answer: ...
		
		    The patient has the following information:
		    {patient2_info}
		"""
		messages = [
		    # {"role": "system", 
		    # "content": "You are a doctor and an expert on deciding if a patient requires a prediabetes care plan or not."},
		    {"role": "user", 
		    "content": prompt3},
		]
	```
---
#### Tree of Thought: Response
		Option A: The patient does not need a prediabetes care plan.
		
		Reasoning: The patient has a healthy lifestyle, no high-risk factors, and is not high-income. These factors do not indicate the need for a prediabetes care plan.
		
		Option B: The patient needs a prediabetes care plan.
		
		Reasoning: The patient has high triglycerides, high blood sugar, and high BMI, which are all risk factors for prediabetes. These factors indicate the need for a prediabetes care plan.
		
		Option C: The patient does not need a prediabetes care plan.
		
		Reasoning: The patient has a healthy lifestyle, no high-risk factors, and is not high-income. These factors do not indicate the need for a prediabetes care plan.
		
		Final Answer: The patient does not need a prediabetes care plan.
	- The correct conclusion was reached!
	- ToT helped more than CoT for this patient
---
# Binary Classification 
## Using embeddings generated via LLM
---
#### Generating Texts from Data
	- For modeling, we need text embeddings
	- First, data must become texts:
			MARITAL: W, RACE: white, ETHNICITY: nonhispanic, GENDER: M, expenses_covered: True, high_income: False, age: 48, Hemoglobin A1c/Hemoglobin.total in Blood: 2.9 %, Body mass index (BMI) [Ratio]: 29.4 kg/m2, Triglycerides: 132.0 mg/dL
/assets/Screenshot 2025-04-09 at 8.50.02 PM.png
size: contain
/assets/Screenshot 2025-04-09 at 8.51.31 PM.png
size: contain
---
#### Generating Embeddings from Texts
	- Now, embeddings can be generated from texts
/assets/Screenshot 2025-04-09 at 8.56.20 PM.png
size: contain
/assets/Screenshot 2025-04-09 at 8.59.33 PM.png
size: contain
---
#### Classification Results
/assets/Screenshot 2025-04-09 at 9.01.09 PM.png
size: contain
	- Logistic Regression results turned out great, almost perfect
---
# Evaluation of Results
	- In-Context Learning
		- Zero-Shot Learning (instructions only, no examples)
			- Did not produce a correct conclusion, but was a great to starting point
		- Chain-of-Thought (include step-by-step reasoning)
			- Helped to reach correct conclusion for Patient #1, but produced some meandering thoughts/hallucinations
		- Few-Shot Learning (include example)
			- (Combined w/ CoT) Produced incorrect conclusion, but improved structure greatly and therefore eliminated hallucinations
		- Tree of Thought (evaluate multiple reasoning paths)
			- Helped (more than CoT) to reach correct conclusion for Patient #2
	- Binary Classification w/ embeddings generated via LLM
		- Achieved near-perfect results w/ simple Logistic Regression
		- Using text embeddings reduces data cleaning and feature engineering steps greatly, but more compute time/resources are needed to generate embeddings
---
# Ideas for Improvement
	- Try larger or medically fine-tuned LLMs
		- These could yield more accurate conclusions with less reliance on prompt engineering
	- Experiment w/ multiple LLMs (2-3)
		- Known as "LLM ensembling," inspired by ML ensembles, which generally produce better results than individual models
	- Try OpenAI APIs or similar cloud-based models
		- To overcome local compute limits and gain access to more powerful, scalable LLMs
---
# Thank You!