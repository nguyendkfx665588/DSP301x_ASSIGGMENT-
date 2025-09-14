Here’s a **README.md** you can include with your project.
It explains what the program does, the data format, and how to run it.

---

### README

#### 1. Overview

This Python program grades multiple-choice exams stored in a plain-text file.
It performs **two grading approaches**:

1. **Pure Python** (Tasks 1–4) – loops and dictionaries
2. **Pandas / Numpy** (Task 5) – fully vectorized analysis

Both approaches:

* Validate each student record
* Compute individual scores (+4 / 0 / –1)
* Print class statistics
* Identify the most skipped and most incorrectly answered questions
* Export a grade file with `StudentID,Score`

---

#### 2. Input File Format

* Each line contains **26 comma-separated values**:

  * 1 student ID followed by **25 answers**.
* Valid student ID matches the pattern:

  ```
  N########    (N + 8 digits, e.g. N00000023)
  ```
* Each answer is one of `A,B,C,D` or blank for skipped.

Example line:

```
N00000023,B,A,D,D,C,B,D,A,C,C,D,B,A,B,A,C,B,D,A,C,A,A,B,D,D
```

---

#### 3. Output Files

1. **Console report**
   Prints:

   ```
   **** ANALYZING ****
   Invalid line of data: ...
   **** REPORT ****
   Total valid lines of data: 21
   Total invalid lines of data: 4
   Total student of high scores: 7
   Mean (average) score: 78.00
   Highest score: 100
   Lowest score: 66
   Range of scores: 34
   Median score: 76
   Question that most people skip: 22 - 6 - 0.29
   Question that most people answer incorrectly: 10 - 5 - 0.24, 18 - 5 - 0.24
   ```

   *Skip/incorrect outputs show
   `QuestionNo – Count – Ratio` (Ratio = count ÷ total students).*

2. **Grades file**
   A new file named `<class>_grades.txt` (pure Python)
   and `<class>_grades (pd).txt` (Pandas)
   Each row contains:

   ```
   N########,Score
   ```

---

#### 4. How to Run

1. Place the Python script and the class text file in the same folder.
2. Open a terminal (or run inside your IDE).
3. Execute:

   ```bash
   python lastname_firstname_grade_the_exams.py
   ```
4. Enter the class name (without “.txt”) when prompted:

   ```
   Enter a class to grade (i.e. class1 for class1.txt): class2
   ```

---

#### 5. Key Libraries

* **re** – Regular expressions for ID validation.
* **statistics** – Mean and median for the pure-Python part.
* **pandas** – DataFrame creation, vectorized scoring, skip/wrong analysis.

---

#### 6. File Structure

```
.
├─ lastname_firstname_grade_the_exams.py
├─ class1.txt   # example input
└─ class2.txt   # example input
```

---

This README provides enough detail for anyone to understand the data format, run the grader, and interpret the results.
