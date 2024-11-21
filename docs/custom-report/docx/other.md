# Context Variables for Vulnerabilities and Report Data

## 1. Total Vulnerabilities
- **Description:**  
  Provides the total count of vulnerabilities associated with a project.
- **Usage Example:**  
  You can display the total number of vulnerabilities for the project.
  
  ```python
  Total Vulnerabilities: {{ totalvulnerability }}
  ```

## 2. Report Type
- **Description:**  
  Specifies the type of report being generated. This can include types like "Audit" or "Re-Audit".
- **Usage Example:**  
  Show the type of the report (Audit or Re-Audit).
  
  ```python
    Report Type: {{ Report_type }}
  ```



## 3. Report Standard
- **Description:**  
  The standard or methodology being used for generating the report (e.g., OWASP, NIST). It will be in the list format, you can use below example to get in a text seperated by comma.
- **Usage Example:**  
  Display the methodology or standard used in the vulnerability report.
  
  ```python
    Report Standard: {{ standard|join(', ') }} 
  ```


## 4. Current Date
- **Description:**  
  Get the current date if needed in the report.

- **Usage Example:**  
  ```python
  Report Generated On: {{ currentdate }}   
  # OR
  {{ currentdate.strftime('%B %d, %Y') }}
  ```

## 5.  Page Break
- **Description:** 
  Allows you to add a page break in the DOCX report, such as after each vulnerability's details.

- **Usage Example:**  
  ```python
    {% if vulnerability %}
    {{ page_break }}
    {% endif %}
  ```

## 6. New Line
- **Description:** 
  Allows you to add a new line if needed in the report.

- **Usage Example:**  
  ```python
    Vulnerability Details:  
    CVSS Score{{ new_line }} 8.8
    
  ```