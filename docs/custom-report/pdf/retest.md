# Context Variable for Project Retests

The `totalretest` variable gives access to the retests associated with a project. It allows you to display details of a project retest, including start and end dates, owners, and status. 

- **Usage Example:**  
  You can loop through `totalretest` to display the retests for a project. However, it is possible to conditionally display the retests based on the `Report_type`, e.g., showing retests only when the report type is "Re-Audit".

  ```html
  <h3>Project Retests:</h3>
    <ul>
        {% for retest in totalretest %}
            <li>{{ retest.startdate }} - {{ retest.enddate }} | Status: {{ retest.status }}

                {% for owner in retest.owner.all %}
                      {{ owner.full_name }}
                    
                {% endfor %}
            </li>
        {% endfor %}
    </ul>
  ```


It is possible to conditionally display the retests based on the `Report_type`, e.g., showing retests only when the report type is "Re-Audit".

  ```html
  {% if Report_type == 'Re-Audit' %}
    <h3>Project Retests:</h3>
    <ul>
        {% for retest in totalretest %}
            <li>{{ retest.startdate }} - {{ retest.enddate }} | Status: {{ retest.status }}

              {% for owner in retest.owner.all %}
                      {{ owner.full_name }}
                    
                {% endfor %}
            </li>
        {% endfor %}
    </ul>
  {% endif %}
  ```