# Customized PDF Report Documentation

APTRS uses the **WeasyPrint** library to generate PDF reports from HTML and CSS. The Django template engine dynamically passes data to the HTML template using context variables. 

### How It Works
The system is designed so you don’t need to edit the Django code that handles report generation unless you are familiar with Django and Python. The recommended approach is to customize the layout and styling by modifying the **HTML** and **CSS** files provided with the project.

### File Structure for Customization
Within the project root directory, there is a subdirectory named `APTRS`. This contains the following important folders for report customization:

1. **Static Folder**:  
    - Located inside the `APTRS` directory.  
    - Contains a `CSS` subfolder where all CSS files for the report are stored.  
    - Includes predefined styles that you can customize to adjust the appearance of the report.

2. **Templates Folder**:  
    - Located in the same directory as the `static` folder.  
    - Contains all the HTML templates used for generating reports.  
    - Templates are modular, with the main report layout in `report.html` and additional reusable components loaded using Django's `{% include %}` tag.

### Key Files for Customization
1. **`report.html`**:
    - The main HTML file for the report.  
    - Includes the overall structure, head tags, and references to CSS files.  
    - Dynamically includes other HTML components for better modularity.

2. **`report-page.css`**:
    - The primary CSS file applied to all report pages.  
    - Handles global styles such as font loading, page numbering, table styles, and default styles for HTML tags.  
    - Defines general formatting applied across the entire report.

3. **Component Files**:
    - Each HTML file has a corresponding CSS file for specific styles.
    - For example, styles specific to an HTML section like `vulnerability.html` will be defined in `vulnerability.css`.
    - This structure ensures easy identification and modification of styles tied to specific sections.

### Fonts

The project uses the **Fira Sans** font by default, stored in the `static/fonts` folder. You can:

  - Add new fonts to the `fonts` folder.
  - Update the CSS in the `report-page.css` file to use your fonts.

### Customization Tips
- Modify **HTML** files to change content layout, add new sections, or reorder components.
- Edit **CSS** files to change colors, fonts, or styles for specific sections.
- Use the modular structure to quickly locate and modify styles for individual components.
- For global styles or formatting (e.g., page numbers, headers, footers), update `report-page.css`.

### Important Notes
- Avoid editing the Django code unless necessary, as it handles the logic for passing variables and generating the PDF.
- All changes to the static files (CSS) or templates (HTML) will reflect in the next report generated.




