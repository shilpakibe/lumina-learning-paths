# Setup Instructions

How to run the Lumina Learning Paths notebooks yourself.

The project was built in **Google Colab** with **Google Drive** for storage. No local installation is needed.

---

## 1. Copy the project to your Google Drive

1. Download this repository (green **Code** button, then **Download ZIP**) and unzip it.
2. In Google Drive, create this folder path: `MyDrive/Portfolio/`
3. Upload the unzipped folder into it and rename it `LuminaLearningGroup`.

The notebooks expect this exact path:

```
/content/drive/MyDrive/Portfolio/LuminaLearningGroup
```

If you use a different path, change the path variables at the top of each notebook.

## 2. Open a notebook in Colab

1. In Drive, open the `notebooks/` folder.
2. Right-click `NB01_synthetic_data_generation.ipynb` and choose **Open with > Google Colaboratory**.
3. Run the first cell. It connects Colab to your Drive (you will be asked to allow access). It is safe to run again; it never deletes anything.

## 3. Install the extra libraries

Colab already has most of what the notebooks need. NB02 installs `gensim` by itself (Step 2.8.2b) if it is missing. If NB02 reports that `sentence-transformers` is missing, run this once in a new cell:

```python
!pip install sentence-transformers
```

The full list is in `requirements.txt`. The first run of NB02 also downloads a small language model (all-MiniLM-L6-v2), so you need an internet connection.

## 4. Run the notebooks in order

| Order | Notebook | What it does |
|---|---|---|
| 1 | `NB01_synthetic_data_generation` | Builds the fictional company and all input tables in `data/raw/` |
| 2 | `NB02_nlp_ksb_course_matching` | Finds skill gaps, matches courses, flags near-duplicates |
| 3 | `NB03_path_generation_recommendation` | Applies the L&D review decisions and builds the learning paths |
| 4 | `NB04_visualization_demo` | Draws the charts in `reports/figures/` |

Use **Runtime > Run all** in each notebook.

## 5. Good to know

- **Same results every time.** A fixed random seed (42) means a re-run produces the same data and the same results.
- **The course catalog is protected.** `data/raw/courses.csv` includes 4 real arXiv abstracts pasted by hand. NB01 does not overwrite it while `REGENERATE_COURSES = False`.
- **The L&D review is included.** NB03 reads the reviewer's decisions from `outputs/curation_review_sheet_completed.csv`. This file was filled in by hand and is never changed by code.
- **Edited a CSV by hand?** Save it as **CSV UTF-8**, otherwise pandas cannot read it.
- **Running outside Colab?** Skip the Drive cell in each notebook, change the path variables to your local folder, and install everything with `pip install -r requirements.txt`.

---

© 2026 Shilpa Kibe. Licensed under CC BY-NC 4.0.
