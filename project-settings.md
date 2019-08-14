# Project Settings
The settings are unique per project and per each template in that project. You'll find the settings in the "More" drop down (next to the **Preview** and the **Produce** buttons).
Click **Settings** and a new popup will show.
> The **Settings** window is only available to Admins and Managers, not to Editors. The Editor should not worry about the settings of the project and focus on the content of the template.
The project settings allows you to control properties on the project level. These settings will take effect when any template in that specific project will be produced.

## Create a project folder
Creates a project folder and places the final rendered file inside that folder. Unless indicated otherwise by the custom naming format or renamed be the user, the name of the folder will be the name of the project.
By default, the folder will be created in the same location as the location the After Effects project. You can change that via the local settings editor and redirect the output to another location (this settings
is not available online due to security reasons). Please refer to the [Unique project settings](/farmer?id=unique-project-settings) in the Farmer documentation.

## Allow users to rename folder
Editors will be able to set the name of the folder manually. You can combine this setting with the custom naming settings if you use `||ProjectName||` as a part of the custom naming format.

## Folder custom naming
Define the format that will be used to name the folder. The custom naming format is defined by using variables that indicate what the name of the folder will be when content is produced.
The variables that you can use are available as small buttons under the custom naming text box (also can be written manually directly in the text box).

For example, lets say you'd like to have your files to be rendered into a folder that has today's date. You custom naming format will look like this:`[[MM_dd]]`.
Basically, this tells the Farmer that when the user will produce content, it should create a folder with a Month, Underscore and Day, so the final folder name will be `08_29` (August 29th).

You can modify the format of the date and the time do have it displayed in many different ways. Refer to [this article](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings)
about the different date and time formats that you can use. Remember to keep the double square brackets around the format.

Another example, lets say that you'd like the rendered content land in a folder with the name of the user that is currently using Template Farm. The custom naming format will look like this:
`||FirstName||_||LastNameInit||`. This custom naming format take the first name of the logged user adds an underscore and their last name initials.
So if John Doe is using Template Farm, all files rendered from this project will land in a folder named `John_D`.

Note that you can add hardcoded values between the variables as well. Like in the example above, we added an underscore between the first name and the last name initials. The hardcoded values are not enclosed
in and brackets or pipes.

> If the Folder with a specific name already exist, the new files will be rendered into the existing folder.

## Folder custom naming reference
| Property Name | Resulting Value
|---|---|
| \|\|ProjectName\|\| |The name of the project (can be entered by the user if the renaming of the project is allowed).|
| \|\|UserName\|\| |The user name of the currently logged in user|
| \|\|FirstName\|\| |The first name of the currently logged in user|
| \|\|FirstNameInit\|\| |The first name initial of the currently logged in user|
| \|\|LastName\|\| |The last name of the currently logged in user|
| \|\|LastNameInit\|\| |The last name initial of the currently logged in user|
| [[hh_mm]] |The specific time in hours and minutes in which the user produces the content (clicks the "Produce" button). The format of the timestamp can be customized according to this [article](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-timespan-format-strings). Remember to keep the double square brackets around the format string.|
| [[MM_dd]] |The specific date in month and day in which the user produces the content (clicks the "Produce" button). The format can be customized according to this [article](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings). Remember to keep the double square brackets around the format string.|

# Template Settings
The template settings allow you to control the properties of each template in the project. These settings will take effect only when the relevant template is produced. Under the template settings, each template will have its own tab, with its own settings.

## Allow Duplicate Templates
Allow users to duplicate the template in order to create multiple versions of the same graphics but with different content. This is also very useful when you want to apply date from an external file like Microsoft Excel spread sheets or a CSV files.

## Allow Template Renaming
Allow users to rename the template. By default the name of the rendered file will be the same as the template name. The renamed template name will be reflected in the final rendered file.
You can combine this setting with the custom naming settings if you use `||TemplateName||` as a part of the custom naming format.

## Template custom naming
Define the format that will be used to name the template and in turn the final rendered files. The custom naming format is defined by using variables that indicate what the name of the template will be when content is produced.
The variables that you can use are available as small buttons under the custom naming text box (also can be written manually directly in the text box).

For example, lets say that you'd like to have your template start with a timestamp (hours and minutes) and followed by the actual template name. The custom naming format will look like this:
`[[hh_mm]]_||TemplateName||`. This custom naming format takes the current time when the user produces the content, add an underscore and the name of the currently produced template. So if a user produces template called **Titles**
at exactly 11am. The template name will be **11_00_Titles**.

The custom naming of the templates also allows you to use values from the template itself as a part of the template name. These values will vary based on the fields that are available in each template. You can use any text field value
from the template as a part of its name. For example, lets say that your template contains two text fields: Title and Subtitle. These values will show up as purple button under the custom naming text box. So if you add there values to
your custom naming format it will look like this `{{Title}}_{{Subtitle}}`. When the user produces the template, the value they entered in the template fields will be applied to the name of the template. So if the user entered
**Hello** in the title field and **There** in the Subtitle field, the resulting name will be the value of **Title** field, and underscore and the value of the **Subtitle** field: **Hello_There**.

> Using the field values of the template as variables in your custom naming is useful when you want to provide more context to what content each rendered file actually contains.

## Template custom naming reference
| Property Name | Resulting Value |
|---|---|
| \|\|ProjectName\|\| |The name of the project (can be entered by the user if the renaming of the project is allowed).|
| \|\|TemplateName\|\| |The name of the template (can be entered by the user if the renaming of the template is allowed).|
| \|\|UserName\|\| |The user name of the currently logged in user|
| \|\|FirstName\|\| |The first name of the currently logged in user|
| \|\|FirstNameInit\|\| |The first name initial of the currently logged in user|
| \|\|LastName\|\| |The last name of the currently logged in user|
| \|\|LastNameInit\|\| |The last name initial of the currently logged in user|
| [[hh_mm]] |The specific time in hours and minutes in which the user produces the content (clicks the "Produce" button). The format of the timestamp can be customized according to this [article](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-timespan-format-strings). Remember to keep the double square brackets around the format string.|
| [[MM_dd]] |The specific date in month and day in which the user produces the content (clicks the "Produce" button). The format can be customized according to this [article](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings). Remember to keep the double square brackets around the format string.|
| {{TEMPLATE TEXT FIELDS}} |The field value to be used as a part of the template name. The amount of these properties varies based on the available fields per each template|