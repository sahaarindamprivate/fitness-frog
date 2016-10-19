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

### Implementing Server-Side Validation

There are two approaches for implementing server side validation rules. Adding errors directly to ModelState and expressing rules using data annotations on models.

#### 1. Using ModelState to Implement Server Side Validation

```
if (ModelState.IsValidField("Duration") && entry.Duration <= 0)
{
	ModelState.AddModelError("Duration", 
		"The Duration field value must be greater than '0'");
}
```

#### 2. Using Data Annotations on Models

Strings are nullable, so add [Required] if the field is required. Value types like int and DateTime are not nullable, so if you want to allow nulls, you need to make them nullable like this: int? or DateTime?

```
[Required]
[MaxLength(200, ErrorMessage = "The Notes field cannot be longer than 200 characters.")]
public string Notes { get; set; }
```

### Displaying Validation Messages

Add a validation summary with some css styling.
```
@Html.ValidationSummary("The following errors were found:", new { @class = "alert alert-danger" })
```

Add validation messages to display for individual fields.
```
@Html.ValidationMessageFor(m => m.ActivityId)
```
To add a global error message to the ModelState, pass an empty string
to the AddModelError method.

```
ModelState.AddModelError("", "This is a global message.");
```

### Implementing Unobtrusive Client-side Validation

Install jQuery.Validation and Microsoft.jQuery.Unobtrusive.Validation

You need to explicitly enable MVC support for client side validation by setting two settings in the Web.config file.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true"/>
    <add key="ClientValidationEnabled" value="true"/>
  </appSettings>
```
Validation Summary section must be rendered inside of the form.

What does unobtrusive mean in this context?  The approach of separating JavaScript code from the page's HTML and following progressive enhancement principles. The page will work even if JavaScript is not enabled.

### Improving Validation CSS Styles

We need to customize Jquery validation library to use the available bootstrap validation styles.

Add new JS file in the scripts folder and reference it in the view.

Here's the JavaScript snippet for jquery.validate.bootstrap.js

``` 
(function ($) {
    var defaultOptions = {
        validClass: 'has-success',
        errorClass: 'has-error',
        highlight: function (element, errorClass, validClass) {
            $(element).closest('.form-group')
                .removeClass(validClass)
                .addClass(errorClass);
        },
        unhighlight: function (element, errorClass, validClass) {
            $(element).closest('.form-group')
                .removeClass(errorClass)
                .addClass(validClass);
        }
    };

    $.validator.setDefaults(defaultOptions);

    $.validator.unobtrusive.options = {
        errorClass: defaultOptions.errorClass,
        validClass: defaultOptions.validClass,
    };
})(jQuery);
```

### Updating the Controller to Handle Updates

Nullabe values need to be cast into their non-nullable counterparts. For example:

```
public ActionResult Edit(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }

            Entry entry = _entriesRepository.GetEntry((int)id);
```
The id parameter is type int?, so when you pass it to the GetEntry method cast it to int.

### Creating a Partial View for the Form

The naming convention for partial views is to prefix the view name with an underscore.

```
@Html.Partial("_EntryForm")
```
Remember to pass only "_EntryForm", not "_EntryForm.cshtml".

### Updating the Controller to Handle Deletes

### Updating the Delete View

Make views strongly typed by using the model directive.

Remember to use 

```
@Model.Date.ToShortDateString()
```
to format the date without time.



### Using TempData


