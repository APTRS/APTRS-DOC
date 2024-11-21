# Context Variables for Company and Users

## 1. Internal Company Name
- **Description:**  

    This variable will provide the name of the internal company (your own company) that is linked to the APTRS. It fetches the company name from the `Company` model where the `internal` field is set to `True`.
  
- **Usage Example:**
  
    ```html
    <p>Company: {{ mycompany }}</p>

    ```


## 2. Project Manager Users
- **Description:**

    This variable gives access to all the internal users who are part of the "Project Manager" group. You can loop through this list to display details like the name of the project managers working on the project.

- **Usage Example:**
  
    ```html
    <h3>Project Managers:</h3>
        <ul>
            {% for user in projectmanagers %}
                <li>{{ user.full_name }} - {{ user.position }}</li>
            {% endfor %}
        </ul>

    ```

## 3. Customer Company Users
- **Description:**

    This variable provides access to the users who belong to the customer’s company for the project. It retrieves the list of active users associated with the project’s customer company.

- **Usage Example:**
  
    ```html
        <h3>Customer Users:</h3>
        <ul>
            {% for user in customeruser %}
                <li>{{ user.full_name }} - {{ user.position }}</li>
            {% endfor %}
        </ul>

    ```