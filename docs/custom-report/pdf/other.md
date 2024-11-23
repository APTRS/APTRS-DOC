# Context Variables for Vulnerabilities and Report Data

## 1. Total Vulnerabilities
- **Description:**  
  Provides the total count of vulnerabilities associated with a project.
- **Usage Example:**  
  You can display the total number of vulnerabilities for the project.
  
  ```html
  <p>Total Vulnerabilities: {{ totalvulnerability }}</p>
  ```

## 2. Severity Count
- **Description:**  
  Shows the count of vulnerabilities that have a severity of Critical or High, Medium, Low, None.
- **Usage Example:**  
  You can display the severity count.
  
  ```html
    <p>Critical Vulnerabilities: {{ critical }}</p>
    <p>High Vulnerabilities: {{ high }}</p>
    <p>Medium Vulnerabilities: {{ medium }}</p>
    <p>Low Vulnerabilities: {{ low }}</p>
    <p>Informational Vulnerabilities: {{ info }}</p>
  ```


## 3. Report Standard
- **Description:**  
  The standard or methodology being used for generating the report (e.g., OWASP, NIST). It will be in the list format, you can use below example to get in a text separated by comma.
- **Usage Example:**  
  Display the methodology or standard used in the vulnerability report.
  
  ```html
    <p>Report Standard: {{ standard|join:", " }}</p>
  ```


## 4. Report Type
- **Description:**  
  Specifies the type of report being generated. This can include types like "Audit" or "Re-Audit".
- **Usage Example:**  
  Show the type of the report (Audit or Re-Audit).
  
  ```html
    <p>Report Type: {{ Report_type }}</p>
  ```

## 5. Chart Image
- **Description:**  
  This variable holds the rendered data for a pie chart showing the distribution of vulnerabilities by severity.
- **Usage Example:**  
  Use this to display a pie chart visualizing vulnerability severity distribution.
  
  ```html
    <div>{{ pie_chart|safe }}</div>
  ```