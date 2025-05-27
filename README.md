# Robot Framework

## 📁 Folder Structure You Have

```
test/
├── CalculatorLibrary.py          # Python file (custom test library)
├── calculator_test.robot         # Robot Framework test file
├── log.html                      # Generated log file
├── report.html                   # Generated summary report
└── output.xml                    # Execution output
```

---

## 🧠 What is Robot Framework?

Robot Framework is a **keyword-driven test automation framework**. It lets you write test cases in human-readable syntax and is widely used for:

* Test automation
* Acceptance testing
* DevOps pipelines (with testing stages)

---

## 🧪 What You Did: Unit Testing a Python Class via Robot Framework

---

### ✅ `CalculatorLibrary.py`

This is a **custom Robot Framework library** that exposes a Python method as a testable keyword.

```python
# CalculatorLibrary.py

class CalculatorLibrary:
    def add(self, a, b):
        return int(a) + int(b)
```

#### 🔍 How it works:

* Robot Framework looks for a **class** (named the same as the file or explicitly mentioned).
* It scans all **public methods** (not starting with `_`) and turns them into **keywords**.
* The method `add()` becomes a **keyword called `Add`** in Robot Framework.

---

### ✅ `calculator_test.robot`

This is your **Robot Framework test case file**, written in `.robot` syntax.

```robot
*** Settings ***
Library    CalculatorLibrary.py
```

* This imports your Python class as a **keyword library**.
* Robot will scan and find the `Add` method as a keyword.

```robot
*** Test Cases ***
Addition Should Work
    ${result}=    Add    5    6
    Should Be Equal As Numbers    ${result}    11
```

#### 🔍 What happens:

1. `Add    5    6` → Calls your Python `add(5, 6)` method.
2. `${result}` stores the return value (in this case, `11`).
3. `Should Be Equal As Numbers` is a **built-in Robot keyword** that compares numbers.
4. The test **passes** if result is `11`, else **fails**.


- calculator_test.robot
  
```robot
*** Settings ***
Library    CalculatorLibrary.py

*** Test Cases ***
Addition Should Work
    ${result}=    Add    5    6
    Should Be Equal As Numbers    ${result}    11

```
---

## ✅ Running the Test

```bash
robot calculator_test.robot
```

This:

* Executes the test
* Creates:

  * `output.xml` → raw test result file
  * `log.html` → detailed step-by-step execution log
  * `report.html` → summary of pass/fail status

You can open these in a browser:

```bash
xdg-open report.html
```

---

## 📈 Why This Is Powerful in DevOps

* You can **unit test your logic** in isolation before pushing to production.
* You can run these tests inside **CI/CD pipelines (Jenkins, GitHub Actions, etc.)**.
* Simple syntax → Easy collaboration between testers and developers.
* Extendable with your own Python logic or use ready-made libraries (e.g. SeleniumLibrary, DatabaseLibrary).

---

