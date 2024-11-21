Once your project is ready, you can generate a report for the identified vulnerabilities. APTRS version 1.0 provides options to generate reports in PDF, Excel, and DOCX formats. Navigate to the report section within the project, select the desired report type and standard, and download the report directly.


![Report](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/report.png)

As of version 1.0, the DOCX report generation feature is experimental. It may be removed or enhanced with more customization options in future versions. Once validated for full customization and consistent presentation, this feature will become permanent.

### Report Customization

You can customize both the PDF and DOCX reports:

- **PDF Reports:** APTRS uses the WeasyPrint Python library to convert HTML into PDF. You can modify the HTML and CSS files used in report generation to match your requirements. 
- **DOCX Reports:** APTRS leverages the `python-docxtpl` library to create DOCX files. The CKEditor WYSIWYG editor is used for entering vulnerability and project details. CKEditor’s HTML output is then converted into DOCX format using the `html2docx` library, and integrated with `docxtpl` to generate the final report.

The HTML and DOCX report templates are available here:
- [HTML and DOCX Report Templates](https://github.com/APTRS/APTRS/tree/main/APTRS/templates)

CSS files for HTML-to-PDF reports can be customized here:
- [CSS for PDF Reports](https://github.com/APTRS/APTRS/tree/main/APTRS/static/css)


#### Report Limitations
- **PDF Reports:** APTRS uses the ``WeasyPrint`` Python library to generate PDF reports. However, ``WeasyPrint`` does not support JavaScript execution, which limits the ability to include custom, interactive charts directly within the PDF. Instead, APTRS generates charts using a Python library and embeds them as static images in the PDF. This approach restricts customization of charts via HTML and CSS.
- **DOCX Reports:** APTRS generates DOCX reports using a combination of ``python-docx``, ``python-docxtpl``, and ``html2docx``. While ``python-docxtpl`` enables dynamic content insertion into documents, it does not support updating or dynamically modifying charts. As a result, charts included in DOCX reports cannot be automatically updated with dynamic values by APTRS.






