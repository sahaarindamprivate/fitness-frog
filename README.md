# fitness-frog
ASP.NET MVC 5 Forms / Treehouse

This repo is for the development of a fitness-frog web app that follows the Treehouse ASP.NET MVC 5 Forms course. The focus is on forms so the data is not persisted--the code for data management is just a work around solution and in no way represents best practices.

Of course, I am using Github as my central repository. It tracks and manages changes--just do it! Version control is a critical part of the development process. Visual Studio Github extension makes it easy to track changes.


### Adding datepicker

[Bootstrap Datepicker Project](https://bootstrap-datepicker.readthedocs.io/en/latest/)

1. Download the zip files. Click the Online Demo link to go to the demo page that has the download button.  

2. Add the necessary CSS and Javascript files to the project. Use the minified css and js. Css is added to the Content folder and javascript is added to the Scripts folder.

3. Update the layout page with references to the CSS and JS files. Order of files is important, but site.css provided by Treehouse is a bundled file that includes bootstrap and bootstrap-datepicker styles so you don't need to add those explicitly in _Layout.cshtml. In a regular project, you would add the links in the <head> or bundle them yourself.

4. Add a Script block to initialize the datepicker in _Layout.cshtml after you add the script tag for bootstrp-datepicker.js

```
<script src="~/Scripts/bootstrap-datepicker.min.js"></script>
<script>
	$('input.datepicker').datepicker({
			format: 'm/d/yyyy'
	});
</script>
```

5. Update the form's Date field. Add the datepicker class to the TextBoxFor method call.

```
@Html.TextBoxFor(m => m.Date, "{0:d}", new { @class = "form-control datepicker" })
```

