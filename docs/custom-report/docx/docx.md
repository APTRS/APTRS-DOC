APTRS provides the ability to generate detailed DOCX reports using customizable templates. You can use Jinja2 template tags within your `.docx` templates to dynamically populate project and vulnerability data. This guide explains how to customize your template and use available context variables.

---

## **Template Location**
The default template is located at: ``/APTRS/templates/report.docx``



You can replace this file with your custom `.docx` template while keeping the required placeholders intact.

---

## **Using Template Tags**
The `.docx` template uses Jinja2 syntax for placeholders and loops. Below are some examples:

### **Placeholders**
- **Syntax:** `{{ variable_name }}`
- **Example:** To display the project name, use: `{{ project.name }}`
     

### **Loops**
- **Syntax:** `{% for item in items %}...{% endfor %}`
- **Example:** To list all vulnerabilities:

```python
{% for vulnerability in vulnerabilities %}

{{ vulnerability.vulnerabilityname }} (CVSS: {{ vulnerability.cvssscore }}) {% endfor %}
```


### **Conditional Statements**
- **Syntax:** `{% if condition %}...{% else %}...{% endif %}`
- **Example:** To check if a project has vulnerabilities:

```python
{% if vulnerabilities %} 
Vulnerabilities are present. 
{% else %} 
No vulnerabilities found. 
{% endif %}
```


---

## **Available Context Variables**


| Variable                 | Description                          |
|--------------------------|--------------------------------------|
| `project`                | The project object for which the report is being generated. Contains all project details.      |
| `owners`                 | User details of All Project Owners                                                             |
| `project_exception`      | Project Exceptions                                                                             | 
| `project_description`    | Project Description                                                                            |
| `vulnerabilities`        | Queryset of vulnerabilities for the project, ordered by CVSS score (higher to lower).          |
| `mycomany`               | The name of the internal (your company) company.                                               |
| `projectmanagers`        | Queryset of users in the "Project Manager" group.                                              |
| `customeruser`           | Queryset of customer users associated with the project's customer company.                     |
| `projectscope`           | Queryset of all project scopes for the project.                                                |
| `totalretest`            | List of retest schedules and owners.                                                           |
| `totalvulnerability`     | Total count of vulnerabilities for the project.                                                |
| `Report_type`            | The type of report being generated (e.g., Audit, Re-Audit).                                    |
| `standard`               | The standard or methodology being used for the report.                                         |
| `currentdate`            | Get Current Date if needed in the report                                                       |
| `page_break`             | Allows you to add page break in the docx report such as page break after each vulnerability details |
| `new_line`               | Allows you to add a new line if needed                                                         |



## **Docx Formatting with Jinja2**

By default, Jinja2 adds blank lines with conditions or loops, which can be problematic for Docx formatting. Unlike HTML, where empty lines have minimal impact, in Docx, this can lead to extra rows in tables or increased table width.

APTRS uses the docxtpl library, based on Python-docx and Jinja2, for template-based rendering. To reduce blank lines, you can use Jinja2's whitespace control.

However, this method doesn't always work flawlessly; it may inadvertently remove important elements like tables along with extra spaces. A simpler solution can involve using a single line of code.

You can modify the code like this:

```python

##Instead of new line for code
{% if vulnerabilities %} 
Vulnerabilities are present. 
{% else %} 
No vulnerabilities found. 
{% endif %}


## you can change it to like this on a single line 

{% if vulnerabilities %} Vulnerabilities are present. {% else %} No vulnerabilities found. {% endif %}
```



## **Color and Conditions**

Each severity level in the report is represented by different colors: low or informational is often green or light blue, while high severity is red. Status can also be color-coded, with fixed vulnerabilities in green and open ones in red or orange.

In HTML reports, you can add severity values to the tag's class attribute and create CSS to apply the colors.

For DOCX reports, colors cannot be applied to undefined elements, limiting customization. To assign colors based on severity—like red for critical and orange for high—include them directly in the conditional statements within your DOCX template.


```python
{% if severity == 'Critical' %} severity with red color. {% elif severity == 'Low' %} severity with green color. {% endif %}
```

![Docx Color](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/docx-color.png)

!!! info "Note"

    
    Adding Jinja2 White Space control to this will remove the color.

What if instead of just color for the text, we want to add color for the cell in the table like below  


<table> 
        <tr> 
            <th>Name</th> 
            <th>Status</th> 
            <th>Severity</th> 
        </tr> 
  
        <tr> 
            <td>SQL Injection</td> 
            <td bgcolor="red">Vulnerable</td> 
            <td bgcolor="green">Confirm Fixed</td> 
        </tr> 
    </table> 

We can do that as well with cellbg in the table, with hex color code like below:

```python
{% cellbg 'FF491C' if vulnerability.vulnerabilityseverity== 'Critical' else 'F66E09' if vulnerability.vulnerabilityseverity== 'High' else 'FBBC02' if vulnerability.vulnerabilityseverity== 'Medium' else '20B803' if vulnerability.vulnerabilityseverity== 'Low' else '3399FF' %}{{ vulnerability.vulnerabilityseverity}}
```

![Docx Color](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/colorbg.png)