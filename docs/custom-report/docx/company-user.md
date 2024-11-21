# Context Variables for Company and Users

## 1. Internal Company Name
- **Description:**  

    This variable will provide the name of the internal company (your own company) that is linked to the APTRS. It fetches the company name from the `Company` model where the `internal` field is set to `True`.
  
- **Usage Example:**
  
    ```python
    Company Who DID the Pentest: {{ mycompany }}

    ```


## 2. Project Manager Users
- **Description:**

    This variable gives access to all the internal users who are part of the "Project Manager" group. You can loop through this list to display details like the name of the project managers working on the project.

- **Usage Example:**
  
    ```python
    Project Managers:
       
            {% for user in projectmanagers %}
                {{ user.full_name }}  {{ user.position }} {{ user.email }}
            {% endfor %}

    ```

## 3. Customer Company Users
- **Description:**

    This variable provides access to the users who belong to the customer’s company for the project. It retrieves the list of active users associated with the project’s customer company.

- **Usage Example:**
  
    ```pthon
        Customer Users:
       
            {% for user in customeruser %}
                {{ user.full_name }} {{ user.position }} {{ user.email }}
            {% endfor %}
        

    ```