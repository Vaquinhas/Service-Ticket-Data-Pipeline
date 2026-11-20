# Mini-Project 1 — Service Ticket Data Pipeline

**Course:** Applied Programming for AI  
**Student Name:** Gonçalo Silva
**Student Number:** 22602710  

## Overview
This repository contains an auditable NumPy/Pandas data-cleaning pipeline for synthetic service desk ticket records (`service_tickets.csv`). It removes exact duplicates, isolates rejected records with full rejection reasons, normalizes categorical fields, applies field-specific numeric rules, builds a standardized feature table, and exports an audit trail without mutating the source dataset.

## Repository Structure
```text
.
├── project_1_student.ipynb   # Main executable notebook (diagnosis, pipeline, edge cases, export)
├── service_tickets.csv       # Unchanged synthetic source dataset
├── REPORT.md                 # Summary of diagnosis, rules, checks, and limitations
├── README.md                 # Project overview and run instructions
└── outputs/
    ├── clean_tickets.csv     # Retained, cleaned tickets and transformation flags
    ├── rejected_tickets.csv  # Rejected raw rows with all applicable rejection reasons
    ├── ticket_features.csv   # Standardized duration and one-hot channel indicators
    └── audit.json            # Row counts, flag totals, and summary statistics
