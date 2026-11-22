# Employee Management System with Job Recommendation

A Django web app for HR teams to manage **departments, positions and employees**, with a
built-in **resume parser** and a **job recommender** that matches a candidate's skills
against a dataset of about 1,900 real job postings.

![Python](https://img.shields.io/badge/Python-3-3776AB?logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-3.2-092E20?logo=django&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-TF--IDF%20%2B%20kNN-F7931E?logo=scikitlearn&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

## Features

**Employee management**

- Login-protected dashboard showing department, position and employee totals
- Create, edit and delete departments and positions (with active/inactive status)
- Employee records: code, name, gender, date of birth, contact, address, email,
  department, position, hire date, salary and status
- Employee profile view

**Resume parsing**

- Upload resumes as PDF or DOCX
- Extract text with `pdfminer` / `docx2txt` and structured fields (name, email,
  skills, experience) with `pyresparser`

**Job recommendation**

- Turns the skills parsed from a resume into character trigrams with a TF-IDF
  vectoriser
- Scores every posting in [`ems/job_final.csv`](ems/job_final.csv) (position, company,
  location, description) by nearest-neighbour distance
- Shows the **top 10 matching jobs** with position, company and location

## How the recommender works

```
resume (PDF/DOCX) ──► text extraction ──► pyresparser ──► skills
                                                           │
job_final.csv ──► stop-word cleaning ──► job descriptions  │
                                              │            ▼
                                              └──► TF-IDF (char 3-grams) + NearestNeighbors
                                                           │
                                                           ▼
                                               top 10 closest postings
```

## Repository layout

```
ems/
  manage.py
  ems/                     # Django project settings and URLs
  employee_information/    # app: models, views, templates
  static/                  # CSS / JS / images
  job_final.csv            # job postings used by the recommender
  Employee Management System presentation.pptx
```

## Run it locally

Python 3.8 is recommended, because `pyresparser` depends on older spaCy releases.

```bash
python -m venv venv
venv\Scripts\activate            # macOS/Linux: source venv/bin/activate
pip install "django==3.2.*" pyresparser spacy==2.3.5 nltk scikit-learn pandas numpy \
            pdfminer.six docx2txt python-docx pymupdf pikepdf ftfy pillow \
            opencv-python matplotlib python-pptx docx2pdf
python -m spacy download en_core_web_sm
python -m nltk.downloader stopwords words

cd ems
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Open http://127.0.0.1:8000/ and sign in with the superuser you created.

## Tech stack

Python · Django · SQLite · scikit-learn (TF-IDF, NearestNeighbors) · pyresparser ·
spaCy · NLTK · pdfminer · Bootstrap

## Possible next steps

- Store uploaded resumes per candidate and keep recommendation history
- Use sentence embeddings instead of character n-grams for semantic matching
- Move the secret key to an environment variable and add tests

## Author

**Darshan Bhavsar** · [LinkedIn](https://www.linkedin.com/in/darshan01/) ·
[GitHub](https://github.com/darshanbhavsar01)

## License

[MIT](LICENSE)
