# Rule Engine with Abstract Syntax Trees (AST)

This project implements a 3-tier rule engine application using **Flask**, **SQLAlchemy**, and **SQLite**. It enables users to create, combine, and evaluate rules using Abstract Syntax Trees (ASTs).

## Getting Started

### Prerequisites

1. Install the required dependencies:

   ```bash
   pip install flask sqlalchemy
   ```

2. Run the Flask application:
   ```bash
   python main.py
   ```

## Components

1. **Backend**: `main.py` - This is the core of the application, built with Flask and SQLAlchemy.
2. **Frontend**: `rlg.py` - Provides a simple user interface using Tkinter.
3. **Automated Test Script**: `test.py` - Runs automated tests on the app using Python's `requests` library.

## Application Features

1. **Create Rule**:
   - Allows users to create individual rules.
   - Each created rule is assigned a unique ID and displayed in the UI.
   - Example: Creating two rules will generate IDs 1 and 2.
2. **Combine Rules**:

   - Users can combine multiple rules into a "mega rule" by providing their IDs in a comma-separated format.
   - The combined rule is assigned a new ID and displayed in the UI.

3. **Evaluate Rule**:
   - Users can evaluate a combined rule by entering the rule ID and the required data in JSON format.

## API Endpoints

1. **Create Rule**

   - **URL**: `/create_rule`
   - **Method**: POST
   - **Data Params** (JSON):
     ```json
     {
       "rule_string": "(age > 30 AND department = 'Sales') OR (salary > 50000)"
     }
     ```
   - **Success Response** (JSON):
     ```json
     {
       "id": 1,
       "ast": "..."
     }
     ```

2. **Combine Rules**

   - **URL**: `/combine_rules`
   - **Method**: POST
   - **Data Params** (JSON):
     ```json
     {
       "rule_ids": [1, 2]
     }
     ```
   - **Success Response** (JSON):
     ```json
     {
       "id": 3,
       "combined_ast": "..."
     }
     ```

3. **Evaluate Rule**

   - **URL**: `/evaluate_rule`
   - **Method**: POST
   - **Data Params** (JSON):
     ```json
     {
       "rule_id": 3,
       "data": {
         "age": 35,
         "department": "Sales",
         "salary": 60000,
         "experience": 6
       }
     }
     ```
   - **Success Response** (JSON):
     ```json
     {
       "result": true
     }
     ```

4. **Modify Rule**
   - **URL**: `/modify_rule`
   - **Method**: POST
   - **Data Params** (JSON):
     ```json
     {
       "rule_id": 1,
       "new_rule_string": "age > 40 AND department = 'HR'"
     }
     ```
   - **Success Response** (JSON):
     ```json
     {
       "message": "Rule updated successfully"
     }
     ```

Let's break down the **curl workflow** and **test script** step by step in a simple way for someone new to Python.

### What is cURL?

`cURL` is a command-line tool that allows you to send HTTP requests to a server and receive responses. It's useful for testing APIs (like the rule engine you're building).

### The Workflow:

1. **Create the First Rule:**

   - The first `curl` command sends a POST request to the server to create a rule.
   - It sends a rule like: `"age > 30 AND department = 'Sales' OR salary > 50000"`.
   - **Command:**
     ```bash
     curl -X POST http://127.0.0.1:5000/create_rule -H "Content-Type: application/json" -d '{"rule_string": "(age > 30 AND department = '''Sales''') OR (salary > 50000)"}'
     ```

2. **Create the Second Rule:**

   - Similarly, this command creates another rule.
   - Rule: `"experience > 5 AND department = 'Marketing'"`.
   - **Command:**
     ```bash
     curl -X POST http://127.0.0.1:5000/create_rule -H "Content-Type: application/json" -d '{"rule_string": "experience > 5 AND department = '''Marketing'''"}'
     ```

3. **Combine the Rules:**

   - This command combines the two rules you just created into one big rule.
   - You pass the IDs of the two rules (which are returned by the server when you create them).
   - **Command:**
     ```bash
     curl -X POST http://127.0.0.1:5000/combine_rules -H "Content-Type: application/json" -d '{"rule_ids": [1, 2]}'
     ```

4. **Evaluate the Combined Rule:**

   - Now you can test this combined rule with user data.
   - The command sends the rule ID (of the combined rule) and user data to check if the user fits the rule.
   - **Command:**
     ```bash
     curl -X POST http://127.0.0.1:5000/evaluate_rule -H "Content-Type: application/json" -d '{ "rule_id": 3, "data": { "age": 35, "department": "Sales", "salary": 60000, "experience": 6 } }'
     ```

5. **Modify a Rule:**
   - This command allows you to modify a rule you already created. For example, you can change the age or department in the rule.
   - **Command:**
     ```bash
     curl -X POST http://127.0.0.1:5000/modify_rule -H "Content-Type: application/json" -d '{ "rule_id": 1, "new_rule_string": "age > 40 AND department = '''HR'''" }'
     ```

---

### Python Test Script:

Now, you have a Python script (`test.py`) to automate these steps, so you don't have to run each `curl` command manually.

#### Here's how the Python script works:

1. **`test_create_rule(rule_string)`**:

   - This function sends a request to create a rule and returns the rule's ID from the server.

2. **`test_combine_rules(rule_id_1, rule_id_2)`**:

   - This function combines two rules by sending their IDs and returns the combined rule's ID.

3. **`test_evaluate_rule(rule_id, data)`**:

   - This function evaluates a rule (combined or single) using the given data.

4. **`test_modify_rule(rule_id, new_rule_string)`**:
   - This function modifies an existing rule by sending a new rule string to replace the old one.

#### Example Code Flow:

1. **Create Rule 1:**
   - The script creates the first rule (`age > 30 AND department = 'Sales'`) and stores the returned rule ID.
2. **Create Rule 2:**
   - It creates the second rule (`salary > 50000 OR experience > 5`) and stores the rule ID.
3. **Combine the Rules:**
   - The script combines these two rules and stores the combined rule's ID.
4. **Evaluate the Combined Rule:**
   - It evaluates this combined rule using sample data (`age: 35`, `department: Sales`, etc.).
5. **Modify a Rule:**
   - Finally, it modifies the first rule (changes the condition to `age > 40 AND department = 'HR'`).

---

### Summary:

- The **curl commands** are used to interact with your rule engine API, sending requests to create, combine, evaluate, and modify rules.
- The **Python test script** automates this process, making it easier to test the rule engine without manually typing curl commands.
- You can run the script (`test.py`) and it will automatically create, combine, and evaluate rules for you!
