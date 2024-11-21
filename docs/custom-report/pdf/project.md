

## Project Model Variables for Template Customization

The `Project` model in APTRS contains the following fields that can be used as template variables in your custom report templates. These variables are passed as part of the context to the report HTML, allowing you to display or manipulate project-related data in your custom reports.

### Available Variables and Their Usage

1. **`project.name`**  
    - Represents the name of the project.  
    - Example usage in HTML:  
      ```html
      <h1>Project Name: {{ project.name }}</h1>
      ```

2. **`project.companyname`**  
    - A foreign key to the `Company` model, representing the company associated (Customer Company) with the project.  
    - Example usage:  
      ```html
      <p>Company Name: {{ project.companyname.name }}</p>
      ```

3. **`project.description`**  
    - A detailed description of the project.  
    - Example usage:  
      ```html
      <p>Description: {{project.description|clean_html}}</p>
      ```
    - It uses CKeditor HTML data, using ``|clean_html`` allow to render html data securely instead of as text.

4. **`project.projecttype`**  
    - Specifies the type of project.  
    - Example usage:  
      ```html
      <p>Project Type: {{ project.projecttype }}</p>
      ```

5. **`project.startdate`**  
    - The start date of the project.  
    - Example usage:  
      ```html
      <p>Start Date: {{ project.startdate }}</p>
      ```
    - You can also modify the date format 
        ```html
            <p>Start Date: {{project.startdate|date:"d/m/Y" }} </p>
        ```

6. **`project.enddate`**  
    - The end date of the project.  
    - Example usage:  
      ```html
      <p>End Date: {{ project.enddate }}</p>
      ```
    - You can also modify the date format 
        ```html
            <p>End Date: {{project.enddate|date:"d/m/Y" }} </p>
        ```

7. **`project.testingtype`**  
    - The type of testing performed for the project (e.g., White Box, Black Box).  
    - Example usage:  
      ```html
      <p>Testing Type: {{ project.testingtype }}</p>
      ```

8. **`project.projectexception`**  
    - Notes or exceptions for the project, if any.  
    - Example usage:  
      ```html
      <p>Exceptions: {{ project.projectexception|clean_html }}</p>
      ```
    - It uses CKeditor HTML data, using ``|clean_html`` allow to render html data securely instead of as text.
    - In most cases if you don't have exceptions and in your report you only this if exception is there you are also do it with conditions.
      ```html
      {% if project.projectexception %}
      <p>Exceptions: {{ project.projectexception|clean_html }}</p>
      {% endif %}
      ```


9. **`project.owner`**  
    - A many-to-many relationship representing users associated as owners of the project.  
    - To display all owners:  
      ```html
      <ul>
        {% for owner in project.owner.all %}
          <li>{{ owner.username }}</li>
          <li>{{ owner.full_name }} </li>
          <td>{{ owner.email }} </li>
          <li>{{ owner.number }} </li>
          <li>{{ owner.position }} </li>
        {% endfor %}
      </ul>
      ```

10. **`project.status`**  
    - The current status of the project, based on `PROJECT_STATUS_CHOICES` (e.g., Upcoming, In Progress, Delay, Completed).  
    - Example usage:  
      ```html
      <p>Status: {{ project.status }}</p>
      ```


