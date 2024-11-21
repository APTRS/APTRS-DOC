# Context Variable for Project Scope

The `projectscope` variable gives access to the scope of a project. It allows you to retrieve and display details of the project's scope, including any specific tasks, requirements, or exceptions related to the project. You can loop through this context variable to show the scope items associated with a project.

- **Usage Example:**
  
  ```python
  Project Scope:
  
      {% for scope in projectscope %}
          {{ scope.scope }}  {{ scope.description }}
      {% endfor %}
  </ul>
  ```
