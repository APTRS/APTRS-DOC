# Project Reports

Generate comprehensive reports for your security assessments in multiple formats. Reports include all vulnerabilities, their details, and project information.

![Report Generation Interface](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/report.png)

## Report Generation

APTRS offers three report formats:

- **PDF**: Professional, polished reports with full formatting
- **Excel**: Spreadsheet format for data analysis
- **DOCX**: Word document format (still experimental in version 2.0)

## Version 2.0 Improvements

- Standards are now saved in project details instead of being selected during report generation
- Streamlined report generation process with fewer steps
- Consistent report formatting across multiple reports for the same project

!!! note "DOCX Status"
    DOCX report generation remains an experimental feature as of version 2.0. It may be enhanced with additional customization options or modified in future releases.

## Report Customization

### PDF Reports
APTRS uses the WeasyPrint Python library to convert HTML into PDF. You can modify the HTML and CSS files to match your requirements.

### DOCX Reports
APTRS leverages `python-docxtpl`, CKEditor WYSIWYG, and `html2docx` to create DOCX files. Vulnerability and project details are entered via CKEditor, converted to DOCX, and integrated with templates.

### Customization Resources

- **Templates**: [HTML and DOCX Report Templates](https://github.com/APTRS/APTRS/tree/main/APTRS/templates)
- **CSS Files**: [CSS for PDF Reports](https://github.com/APTRS/APTRS/tree/main/APTRS/static/css)

## Report Limitations

### PDF Reports
- WeasyPrint doesn't support JavaScript execution
- Charts are generated as static images instead of interactive elements
- Limited customization of charts via HTML/CSS

### DOCX Reports
- Uses combination of `python-docx`, `python-docxtpl`, and `html2docx`
- Cannot automatically update or modify charts with dynamic values






