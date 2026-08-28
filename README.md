---
projectName: "sqlite-demo"
author: "Klaus Prinz"
domain: "learning"
status: "active"
createdDate: "2026-08-28"
tags: [sqlite, database, jupyter notebook, demo, presentation, markdown, marp]
requester: "bibinfo"
ticketId: ""
---

# sqlite-demo

> **Quick Summary:** A zero-configuration SQLite database demonstration and interactive Jupyter notebook designed to showcase relational data management using library-themed datasets.

---

## Quick Start & How to Run

1. **Environment:** Ensure you are using the `sqlite-demo` Conda environment (to create run `conda env create -f environment.yaml`).
2. **Setup:** Activate the environment via `conda activate sqlite-demo`.
3. **Inputs:** Sample library data is pre-populated inside the local `data/` directory.
4. **Execution:** Open `notebooks/sqlite_demo.ipynb` and run cells sequentially.

---

## Context & Objectives
Demonstrate the lightweight, serverless power of SQLite for rapid prototyping and local data storage. Provide a hands-on Python/Pandas playground to explore relational queries. Introduce Object-Relational Mapping (ORM) concepts using SQLAlchemy at a beginner-friendly level.

---

## Project Layout

```text
.
├── assets/                    # Screenshots, process diagrams, and images
│   └── screenshots/
├── data/                      # Local sample SQLite databases and CSVs
├── notebooks/
│   └── sqlite_demo.ipynb      # Main interactive tutorial notebook
├── presentation/              # Markdown presentation source and exports
├── environment.yaml           # Conda environment configuration
└── README.md
```

---

## Documentation & Visuals

- Slides and presentation materials are stored under the presentation directory.

---

## Dependencies & Requirements

- Core Package: Python 3.13, Pandas, and SQLAlchemy managed via Conda.

- Database Engine: Built-in Python sqlite3 module (no external server required).

---

## Notes & Future TODOs

---