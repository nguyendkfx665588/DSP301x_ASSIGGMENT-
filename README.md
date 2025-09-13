# DSP301x_ASSIGGMENT-
Assignment of Data Science Course at FUNIX
# README — Exam Grader (Tasks 1–5)

## Overview

This program grades a 25-question multiple-choice exam, validates each line in the input file, computes per-student scores using a fixed answer key, prints class statistics, and writes a grades file. It implements two parallel approaches:

* **Tasks 1–4 (regex + statistics, no pandas)** — validation, scoring, class stats, and writing `<input>_grades.txt`.
* **Task 5 (pandas)** — repeats the analysis using two DataFrames (**answer\_df** for the key and **students\_df** for valid student answers), and writes `<input>_grades.txt` again.

> ⚠️ You asked not to change the code. This README documents the current behavior exactly as the code implements it—including prompts, outputs, and edge cases.

---

## Input format

* At program start (**Task 1**), you’re prompted for a base filename (e.g., `class1`). The program appends `.txt` automatically and opens `class1.txt`.
* Each **non-blank** line should have **exactly 26 comma-separated values**:

  1. **Student ID**: `N########` (capital `N` followed by exactly **8 digits**)
  2. **25 answers**: each `A/B/C/D` or **empty** for skipped

**Example line**

```
N00000001,B,A,D,D,C,B,D,A,C,C,D,B,A,B,A,C,B,D,A,C,A,A,B,D,D
```

---

## Validation rules (Task 2)

For each line in the input file:

* If the line **does not** contain exactly 26 values → prints:

  ```
  Invalid line of data: does not contain exactly 26 values:
  <the original line>
  ```
* Else if the **ID** is not in the format `^N\d{8}$` → prints:

  ```
  Invalid line of data: N# is invalid
  <the original line>
  ```
* Otherwise, the line is **valid** and will be used for scoring.

At the end of analysis, the script prints:

```
**** REPORT ****
Total valid lines of data: X
Total invalid lines of data: Y
```

If there are no invalid lines, it prints `No errors found!` before the report.

> ℹ️ In Task 2, the code calculates `total_student = len(lines)` (all lines read), not the number of valid lines. Some ratios later in Task 2 use this `total_student`.

---

## Scoring rules (Tasks 3 & 5)

* **+4** for each **correct** answer
* **0** for each **blank** (skipped) answer
* **−1** for each **incorrect** answer

Maximum score per student: **100** (25 × 4).

### High score threshold

* **Tasks 3 (no pandas):** “high scores” are **strictly greater than 80** (`> 80`).
* **Task 5 (pandas):** “high scores” are **greater than or equal to 80** (`>= 80`).

This difference is intentional in your code and documented here as-is.

---

## Console output

### After scoring (Tasks 3 & 5)

The program prints class statistics:

* Total student of high scores
* Mean (average) score
* Highest, Lowest, Range, Median

### Most skipped / Most incorrect

* **Tasks 3 (no pandas):**

  * Skipped ratio uses `total_student = len(lines)` (all read lines)
  * Most incorrect ratio also uses `total_student`
* **Task 5 (pandas):**

  * Skipped & incorrect ratios use only the **valid** students (`len(students_df)`)

Printed format (both sections) is:

```
Question that most people skip: <question_number> - <count> - <ratio>
Question that most people answer incorrectly: <question_number> - <count> - <ratio>
```

---

## Output file(s)

### Task 4 (no pandas)

* Builds `<input>_grades.txt` from the first filename you entered.
* Writes **one line per (ID, score)** for all lines in the file where `len(sid) == 9`:

  ```
  N00000001,73
  N00000002,86
  ...
  ```
* Uses **file mode `"x"`** (create; **fail if exists**).
  The code currently catches **`FileNotFoundError`**, not `FileExistsError`.
  If the file already exists, Python raises **`FileExistsError`** and the program will **terminate** (uncaught).

  * **Workaround:** delete the existing `<input>_grades.txt` before re-running.

### Task 5 (pandas)

* Prompts **again** for the filename (e.g., type `class1` again).
* Writes **again** to the same `<input>_grades.txt`, now using **file mode `"w"`** (always **overwrite**).
* Format is the same: `ID,score` lines, based on the pandas-computed `scores` Series.

> ⚠️ Net effect:
>
> * Task 4 **creates** the grades file (fails if it already exists).
> * Task 5 **overwrites** the grades file unconditionally.
> * If both sections run in one execution, the final file reflects the **pandas** scores.

---

## How to run

1. Ensure your data file (e.g., `class1.txt`) is in the same folder as the script.
2. Install **pandas** if you want Task 5 to run:

   ```
   pip install pandas
   ```
3. Run the script:

   ```
   python lastname_firstname_grade_the_exams.py
   ```
4. When prompted:

   * Enter the base filename (e.g., `class1`) for **Task 1**.
   * Later, for **Task 5**, the script **prompts again**; enter the same base name (e.g., `class1`).

---

## Notes & gotchas (as coded)

* **Two prompts for filename**: Task 1 and Task 5 both ask for it. This is expected with the current code; enter the same name both times.
* **Different “high score” thresholds**: `> 80` in Tasks 3 vs. `>= 80` in Task 5.
* **Ratios base differ**:

  * Task 3 uses **all read lines** (including invalid/blank) for ratio denominators.
  * Task 5 uses **only valid students**.
* **File writing behavior**:

  * Task 4: `"x"` (create; fails if file exists). The code catches the wrong exception type; if the file exists, you’ll see an unhandled `FileExistsError`.
  * Task 5: `"w"` (overwrite).
* **Answer key** is hard-coded and must match exactly 25 questions.
* **Skipping** is identified by empty strings (`""`).

---

## Example

**Input** (`class1.txt`):

```
N00000001,B,A,D,D,C,B,D,A,C,C,D,B,A,B,A,C,B,D,A,C,A,A,B,D,D
N00000002,B,A,D,D,C,B,D,A,C,C,D,B,A,,A,C,B,D,A,C,A,A,B,D,D
N00000003,B,A,D,,C,B,D,A,C,C,D,B,A,B,A,C,B,D,A,C,A,A,B,D,D
```

**Outputs** (console):

* Validation report
* Class statistics
* Most skipped / most incorrect questions

**Output file**:

* `class1_grades.txt` containing:

  ```
  N00000001,100
  N00000002,96
  N00000003,95
  ```

  (Scores will vary with your actual data and the skip/wrong logic.)

---

## Requirements

* **Python** 3.9+
* **Standard library**: `re`, `statistics`
* **Third-party (Task 5)**: `pandas`

---

## License & authorship

* Educational assignment code; author your name/ID as required by your course.
