

## Project Model Variables for Template Customization

The `Project` model in APTRS contains the following fields that can be used as template variables in your custom report templates. These variables are passed as part of the context to the report Docx, allowing you to display or manipulate project-related data in your custom reports.

### Available Variables and Their Usage

1. **`project.name`**  
    - Represents the name of the project.  
    - Example usage in HTML:  
      ```python
      Project Name: {{ project.name }}
      ```

2. **`project.companyname`**  
    - A foreign key to the `Company` model, representing the company associated (Customer Company) with the project.  
    - Example usage:  
      ```python
      Company Name: {{ project.companyname.name }}
      ```

3. **`project_exception`**  
    - A detailed description of the project.  
    - Example usage:  
      ```python
      Description: {{p project_exception}}
      ```
    - It uses CKeditor HTML data, its converted into the docx format from HTML, hence its required to use **`p`** at the start of the tag.

4. **`project.projecttype`**  
    - Specifies the type of project.  
    - Example usage:  
      ```python
      Project Type: {{ project.projecttype }}
      ```

5. **`project.startdate`**  
    - The start date of the project.  
    - Example usage:  
      ```python
      Start Date: {{ project.startdate }}
      ```
    - You can also modify the date format 
        ```python
            Project Start Date: {{ project.startdate.strftime("%d/%m/%Y") }}
        ```

6. **`project.enddate`**  
    - The end date of the project.  
    - Example usage:  
      ```python
      <End Date: {{ project.enddate }}
      ```
    - You can also modify the date format 
        ```python
            Project End Date: {{project.enddate.strftime("%d/%m/%Y") }}
        ```

7. **`project.testingtype`**  
    - The type of testing performed for the project (e.g., White Box, Black Box).  
    - Example usage:  
      ```python
      Testing Type: {{ project.testingtype }}
      ```

8. **`project_exception`**  
    - Notes or exceptions for the project, if any.  
    - Example usage:  
      ```python
      Exceptions: {{p project_exception }}
      ```
    - It uses CKeditor HTML data, its converted into the docx format from HTML, hence its required to use **`p`** at the start of the tag.
    - In most cases if you don't have exceptions and in your report you only this if exception is there you are also do it with conditions.
      ```python
      {% if project_exception %}
      Exceptions: {{p project_exception }}
      {% endif %}
      ```


9. **`project.owner`**  
    - Note: This is not available, due to issues with the method provided by django to loop many to many database relation, it does not work with the jinja2. 
      

10. **`project.status`**  
    - The current status of the project, based on `PROJECT_STATUS_CHOICES` (e.g., Upcoming, In Progress, Delay, Completed).  
    - Example usage:  
      ```python
      Status: {{ project.status }}
      ```


In order to get Project owner details, APTRS provide a seperate context and tag `owners` which contain user details for project owners. In order to use it you have to loop through details like below:

```python
      {% for owner in owners %}
        {{ owner.full_name}}
        {{ owner.email}}
        {{ owner.position}}
        {{ owner.number}}
      {% endfor %}
```