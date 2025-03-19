# AI-powered Resume-Analyzer


# Import required libraries
import spacy
import pandas as pd
import re

# Load spaCy's small English language model
nlp = spacy.load("en_core_web_sm")


# Step 3: Function to load resume from text file
def load_resume(file_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        resume_text = file.read()
    return resume_text


# Step 4: Function to extract contact information (email and phone)
def extract_contact_info(text):
    # Regex to extract email
    email = re.findall(r'\S+@\S+', text)

    # Regex to extract phone number
    phone = re.findall(r'(\+?\d{1,3}[-.\s]?\(?\d{1,4}?\)?[-.\s]?\d{1,4}[-.\s]?\d{1,4}[-.\s]?\d{1,9})', text)

    return {
        "Email": email[0] if email else None,
        "Phone": phone[0] if phone else None
    }


# Step 5: Function to extract education information
def extract_education(text):
    education_keywords = ['Bachelor', 'Master', 'BSc', 'MSc', 'PhD', 'University']

    # Extract sentences mentioning education-related keywords
    education = [sent.text for sent in nlp(text).sents if any(keyword in sent.text for keyword in education_keywords)]

    return education


# Step 6: Function to extract skills
def extract_skills(text):
    # Define a list of skill keywords you're looking for
    skill_keywords = ['Python', 'Java', 'SQL', 'Machine Learning', 'AI', 'Data Science', 'NLP']

    # Extract skills by checking if any of the keywords appear in the resume text
    skills = [keyword for keyword in skill_keywords if keyword.lower() in text.lower()]

    return skills


# Step 7: Function to summarize all extracted resume data
def summarize_resume(contact_info, education, skills):
    summary = {
        "Contact Information": [contact_info],
        "Education": [education],
        "Skills": [skills]
    }
    return pd.DataFrame(summary)


# Main program execution

# Step 3: Load resume from file

resume_text = load_resume(r"C:\Users\rakhi\Downloads\Resume.txt")  # Replace with the path to your resume file

# Step 4: Extract contact information
contact_info = extract_contact_info(resume_text)
print("Contact Information:", contact_info)

# Step 5: Extract education information
education = extract_education(resume_text)
print("Education:", education)

# Step 6: Extract skills
skills = extract_skills(resume_text)
print("Skills:", skills)

# Step 7: Summarize the resume information
resume_summary = summarize_resume(contact_info, education, skills)

# Display the summary in DataFrame format
print(resume_summary)
