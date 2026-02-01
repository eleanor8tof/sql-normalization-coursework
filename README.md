# SQL & Database Normalization Project (PostgreSQL)

## Overview

This project demonstrates my understanding of **relational database design**, **normalization up to Fourth Normal Form (4NF)**, and **analytical SQL querying** using PostgreSQL.

Starting from a denormalized lecturer dataset containing multivalued attributes, I analyzed data redundancy and anomalies, identified **functional dependencies (FDs)** and **multivalued dependencies (MVDs)**, and redesigned the schema into a clean 4NF structure. I then rewrote analytical SQL queries on the normalized schema to ensure correctness, efficiency, and readability.

This project is presented as part of my data and database learning portfolio and reflects practical database modeling and querying skills.

---

## Skills & Tools

* SQL (PostgreSQL)
* Database Normalization: 1NF → 2NF → 3NF → BCNF → 4NF
* Multivalued Dependencies (MVD)
* Schema Decomposition
* Primary / Foreign Keys
* Common Table Expressions (CTEs)
* Aggregations & Joins
* pgAdmin 4

---

## Problem Background

The original relation stored lecturer information together with the courses they teach and the languages they speak in a single table.

Because **LANGUAGE** and **TEACHES** are independent multivalued attributes, the schema suffered from:

* Insertion anomalies
* Update anomalies
* Deletion anomalies
* Significant data redundancy

This made the relation inefficient and hard to maintain as the dataset grew.

---

## Normalization Analysis

### Maximum Normal Form of Original Schema

The original relation satisfies **1NF** (all attributes are atomic) but violates **2NF**, due to partial dependencies of non-prime attributes on **SSN**.

### Key Dependencies Identified

**Functional Dependency (FD):**
SSN → SURNAME, ADDRESS, SALARY, DEPARTMENT

**Multivalued Dependencies (MVDs):**
SSN →→ LANGUAGE
SSN →→ TEACHES

Because **SSN** is not a superkey and there exist non-trivial MVDs, the relation violates **4NF**.

---

## 4NF Decomposition Design

To eliminate redundancy and anomalies, the schema was decomposed into three relations:

### RA — Lecturer Information

**Attributes:** SSN, SURNAME, ADDRESS, SALARY, DEPARTMENT
**Primary Key:** SSN
**Normal Form:** BCNF

### RL — Lecturer Languages

**Attributes:** SSN, LANGUAGE
**Primary Key:** (SSN, LANGUAGE)
**Foreign Key:** SSN → RA(SSN)
**Normal Form:** 4NF

### RT — Lecturer Courses

**Attributes:** SSN, TEACHES
**Primary Key:** (SSN, TEACHES)
**Foreign Key:** SSN → RA(SSN)
**Normal Form:** 4NF

This design separates independent multivalued facts while preserving data integrity through key constraints.

### Schema Overview (4NF Decomposition)

> Visual overview of the final normalized schema.

![Schema Overview](assets/schema_4nf_overview.png)

---

## Representative SQL Queries

Only selected queries are included to highlight design and querying skills.

### Courses Taught per Lecturer

Counts the number of courses taught by each lecturer using the normalized schema.

**Key point:**
Redundancy-free design allows direct aggregation without `DISTINCT`.

---

### Lecturers Speaking More Than Two Languages and Teaching Databases

Uses **multiple CTEs** to:

* Aggregate language counts
* Filter lecturers teaching a specific course
* Join normalized relations cleanly

**Key point:**
Demonstrates layered SQL logic and correct joins across 4NF tables.

---

### Department-Level Analysis with Multiple Conditions

Identifies departments with:

* More than two lecturers
* Lecturers earning above a salary threshold
* Lecturers speaking more than two languages

Implemented using **CTEs** for clarity and maintainability.

**Key point:**
Shows how normalization improves query structure and analytical reasoning.

---

## Why This Design Matters

* Eliminates data redundancy
* Prevents update, insertion, and deletion anomalies
* Improves query correctness and readability
* Scales better as data volume increases

This project reflects how theoretical normalization concepts translate into practical database design and SQL implementation.

---

## How to Run (Optional)

* Database: PostgreSQL
* Tool: pgAdmin 4
* Create tables using the schema definitions
* Execute queries using the normalized relations

---

## Notes

* This project focuses on **design clarity and reasoning**, not dataset size
* All identifiers are anonymized
* The repository presents selected components rather than a full coursework submission

