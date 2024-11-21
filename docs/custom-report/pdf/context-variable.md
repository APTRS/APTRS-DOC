

This document describes how to customize the HTML templates used to generate PDF reports using the `GetHTML` function in your Django application. Below are the available context variables that can be used in the HTML templates and their respective details.

---

## Context Variables

The following context variables are passed to the HTML template (`report.html`) during PDF generation. Users can use these variables to dynamically customize the content of the PDF report.

| Variable Name       | Description                                                                                 |
|---------------------|---------------------------------------------------------------------------------------------|
| `project`           | The project object for which the report is being generated. Contains all project details.   |
| `vuln`              | Queryset of vulnerabilities for the project, ordered by CVSS score (higher to lower).       |
| `instances`         | Queryset of all vulnerable instances associated with the project.                           |
| `projectmanagers`   | Queryset of users in the "Project Manager" group.                                           |
| `customeruser`      | Queryset of customer users associated with the project's customer company.                  |
| `mycomany`          | The name of the internal (your company) company.                                            |
| `projectscope`      | Queryset of all project scopes for the project.                                             |
| `totalretest`       | Queryset of all retests for the project.                                                    |
| `totalvulnerability`| Total count of vulnerabilities for the project.                                             |
| `ciritcal`          | Count of vulnerabilities with severity "Critical".                                          |
| `high`              | Count of vulnerabilities with severity "High".                                              |
| `medium`            | Count of vulnerabilities with severity "Medium".                                            |
| `low`               | Count of vulnerabilities with severity "Low".                                               |
| `info`              | Count of vulnerabilities with severity "Informational" or "None".                           |
| `standard`          | The standard or methodology being used for the report.                                      |
| `Report_type`       | The type of report being generated (e.g., Audit, Re-Audit).                                 |
| `pie_chart`         | Rendered pie chart data showing the vulnerability distribution by severity.                 |

---

## How to Customize Templates

1. **Locate the HTML Template**: 
    - The report template is located at `templates/*`. You can modify this file or create a new one for different report types.

2. **Use Context Variables**: 
    - Insert the context variables into the template using Django's template syntax. For example:
        ```html
        <h1>Vulnerability Report for {{ project.name }}</h1>
        <p>Total Vulnerabilities: {{ totalvulnerability }}</p>
        <p>Critical Issues: {{ ciritcal }}</p>
        <p>High Severity Issues: {{ high }}</p>
        ```





3. **Dynamic Content**:
    - Use loops and conditionals to dynamically render data. For example:
        ```html
        <ul>
        {% for vuln in vuln %}
            <li>{{ vuln.vulnerabilityname }} - Severity: {{ vuln.vulnerabilityseverity }}</li>
        {% endfor %}
        </ul>
        ```

4. **Styling and Charts**:
    - Use `{{ pie_chart|safe }}` to embed the rendered pie chart in the HTML. Ensure that the `safe` filter is applied to include raw HTML safely.

---

## Example Template Usage

Here’s a sample section of the `report.html` template:

```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ project.name }} Vulnerability Report</title>
</head>
<body>
    <h1>{{ mycomany }} - Vulnerability Report</h1>
    <h2>Project: {{ project.name }}</h2>
    <p>Total Vulnerabilities: {{ totalvulnerability }}</p>
    <p>Critical Issues: {{ ciritcal }}</p>
    <p>High Severity Issues: {{ high }}</p>
    <p>Medium Severity Issues: {{ medium }}</p>
    <p>Low Severity Issues: {{ low }}</p>
    <p>Informational Issues: {{ info }}</p>

    <h3>Vulnerability Breakdown</h3>
    {{ pie_chart|safe }}

    <h3>Vulnerabilities</h3>
    <ul>
        {% for v in vuln %}
            <li>{{ v.vulnerabilityname }} - Severity: {{ v.vulnerabilityseverity }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```