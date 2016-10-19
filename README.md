# fitness-frog
ASP.NET MVC 5 Forms / Treehouse

This repo is for the development of a fitness-frog web app that follows the Treehouse ASP.NET MVC 5 Forms course. The focus is on forms so the data is not persisted--the code for data management is just a work around solution and in no way represents best practices.

Of course, I am using Github as my central repository. It tracks and manages changes--just do it! Version control is a critical part of the development process. Visual Studio Github extension makes it easy to track changes.


### Adding datepicker

[Bootstrap Datepicker Project](https://bootstrap-datepicker.readthedocs.io/en/latest/)

1. Download the zip files. Use the minified css and js.

2. Add the necessary CSS and Javascript files to the project.

3. Update the layout page with references to the CSS and JS files. Order of files is important, but site.css is a bundled file that includes bootstrap and bootstrap-datepicker styles so you don't need to add those explicitly in _Layout.cshtml.

4. Add a Script block to initialize the datepicker in _Layout.cshtml after you add the script tag for bootstrp-datepicker.js

``` html
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