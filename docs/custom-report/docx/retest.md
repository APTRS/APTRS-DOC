# Context Variable for Project Retests

The `totalretest` variable gives access to the retests associated with a project. It allows you to display details of a project retest, including start and end dates, owners, and status. 

- **Usage Example:**  
  You can loop through `totalretest` to display the retests for a project. However, it is possible to conditionally display the retests based on the `Report_type`, e.g., showing retests only when the report type is "Re-Audit".

  ```python
    Project Retests:
    
        {% for retest in totalretest %}
            {{ retest.startdate }}  {{ retest.enddate }} | Status: {{ retest.status }}

                    {% for owner in retest.owners %}
                        {{ owner }}
                    {% endfor %}

        {% endfor %}
    
  ```


!!! info "Note"

    
    The ``{{ owner }}`` will only show the full_name of the owner, as of now its limited to full name only.


It is possible to conditionally display the retests based on the `Report_type`, e.g., showing retests only when the report type is "Re-Audit".

  ```html
  {% if Report_type == 'Re-Audit' %}
    Project Retests:

        {% for retest in totalretest %}
            {{ retest.startdate }} - {{ retest.enddate }} | Status: {{ retest.status }}

                  {% for owner in retest.owners %}
                        {{ owner }}
                    {% endfor %}

        {% endfor %}

  {% endif %}
  ```