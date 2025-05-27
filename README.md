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
class CalculatorLibrary:
    def add(self, a, b):
        return int(a) + int(b)

    def subtract(self, a, b):
        return int(a) - int(b)

    def multiply(self, a, b):
        return int(a) * int(b)

    def divide(self, a, b):
        if int(b) == 0:
            return "Cannot divide by zero"
        return int(a) / int(b)
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

Subtraction Should Work
    ${result}=    Subtract    10    4
    Should Be Equal As Numbers    ${result}    6

Multiplication Should Work
    ${result}=    Multiply    3    7
    Should Be Equal As Numbers    ${result}    21

Division Should Work
    ${result}=    Divide    20    4
    Should Be Equal As Numbers    ${result}    5.0

Division By Zero Should Fail Gracefully
    ${result}=    Divide    5    0
    Should Be Equal    ${result}    Cannot divide by zero
```
---

## ✅ Running the Test

```bash
robot calculator_test.robot

(myenv) root@zssa:~/ROBOT/test# robot calculator_test.robot
==============================================================================
Calculator Test                                                               
==============================================================================
Addition Should Work                                                  | PASS |
------------------------------------------------------------------------------
Subtraction Should Work                                               | PASS |
------------------------------------------------------------------------------
Multiplication Should Work                                            | PASS |
------------------------------------------------------------------------------
Division Should Work                                                  | PASS |
------------------------------------------------------------------------------
Division By Zero Should Fail Gracefully                               | PASS |
------------------------------------------------------------------------------
Calculator Test                                                       | PASS |
5 tests, 5 passed, 0 failed
==============================================================================
Output:  /root/ROBOT/test/output.xml
Log:     /root/ROBOT/test/log.html
Report:  /root/ROBOT/test/report.html


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



---

## ✅ Option 1: Transfer Files to Your Local Machine

This is the **most recommended and secure** approach.

### 📥 Step-by-step:

1. On **your local machine** (the one you’re using to SSH into the Linux CLI machine), run:

   ```bash
   scp root@<server-ip>:/root/ROBOT/test/report.html .
   scp root@<server-ip>:/root/ROBOT/test/log.html .
   ```

   Replace `<server-ip>` with the actual IP of your Linux server.

2. Once the files are copied, **open them with your browser** on your local machine:

   * Double-click `report.html` or
   * Open with:

     ```bash
     firefox report.html
     # or
     google-chrome report.html
     ```

---

## ✅ Option 2: Serve Files with Python (View in Browser via IP)

If you can’t or don’t want to use `scp`, you can expose the folder temporarily via HTTP.

### Step-by-step:

1. On the Linux CLI machine, navigate to the folder:

   ```bash
   cd /root/ROBOT/test
   ```

2. Run a temporary web server:

   ```bash
   python3 -m http.server 8080
   ```

3. On your **local browser**, open:

   ```
   http://<server-ip>:8080/report.html
   ```

   or

   ```
   http://<server-ip>:8080/log.html
   ```

> ⚠️ Ensure that:
>
> * Port 8080 is open in the firewall/security group.
> * You're on the same network or have VPN access to the server if it's in a cloud or data center.

---
