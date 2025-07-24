#  Course Recommendation API

API ini menyediakan rekomendasi kursus berdasarkan keyword, menggunakan data yang sudah diproses dan disimpan dalam format JSON.

---

##  Teknologi
- Python 3
- FastAPI
- Uvicorn
- JSON

---

## Struktur Proyek

course_api_project/
├── main.py # File utama FastAPI
├── recommendations_all_combinations.json # Dataset preprocessed
├── requirements.txt # Dependencies
└── README.md # Dokumentasi ini

---

##  Cara Menjalankan Lokal

1. *Clone repo ini*:
```bash
git clone https://github.com/username/course_api_project.git
cd course_api_project
Install dependencies:

pip install -r requirements.txt
Jalankan server:

uvicorn main:app --reload
Akses API di browser:

Swagger UI: http://localhost:8000/docs

Endpoint contoh:
http://localhost:8000/recommendations/?keyword=python

## Endpoint
/recommendations/?keyword=...
Method: GET
keyword: string keyword pencarian kursus

Contoh Response:
json

{
  "status": "success",
  "keyword": "python",
  "data": [
    {
      "python data science": [
        {
          "title": "Python for Data Science",
          "platform": "Coursera",
          "url": "https://example.com"
        }
      ]
    }
  ]
}
## Ringkasan 
Endpoint Utama: /recommendations/?keyword=...
Contoh: http://localhost:8000/recommendations/?keyword=python
Data: recommendations_all_combinations.json (hasil pre-processing)

#Kontak
Dibuat oleh tim MLOps G6 SISTECH 2025