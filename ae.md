# Creating Templates in After Effects
If you are running the Farmer for the first time, you need to setup the Farmer to work with your version of After Effects.
You learn how to do that [here](http://videoflow.io/documentation/farmer?id=setup-after-effects).

Templates are created directly inside of After Effects project by starting the name of the compositions and layers that you want to include in your template with a hashtag (#).
If you start the name of a composition with a # it will become a template, if you start the name of a text layer with a # it will become a text field, and so on.
Refer to the documentation below to get the full list of the tags that you can use.

## Create a Template
Starting the name of a composition with #, will include the composition as a template. This composition will be shown as a template in Template Farm and all the layers
that have their name start with a # will be presented as fields of the template.
```Syntax
#[Template Name]
```
```Example
#MyTemplate
```

## Text Fields
Starting the name of a text layer with # will include the layer as a text field in the template.
```Syntax
#[Field Name]
```
```Example
#MyTitle
```

## Images
Starting the name of a footage layer or a composition layer with # will display the layer as an image place holder in template.
This will allow you to assign images to the field either via an upload or by pasting URLs.
```Syntax
#[Field Name]
```
```Example
#MyImage
```

## Expression Controls
Starting the name of an expression control that is applied to any layer within the composition with # will display the control in the template.
A slider control will be shown as a slider, and a color picker as a color picker.
Using the expression controls allows you to control any property within the composition,
even if it is not a layer but a sub-property of a layer.
For example: you can control the position of a layer by connecting its position to the slider by an expression.
```Syntax
#[Property Name]
```
```Example
#MySlider
```

# Advanced Tags
You can add a more advanced functionality to your templates by using the advanced tags. Advanced tags are used to ensure that your templates perform in the best way possible,
by protecting fields from not being populated or having too many characters, previewing multiple frames to ensure maximum quality, and many more.

## Previews
Setting frame 50 and 100 (as in the example) of the composition as the frames that will be rendered when the user clicks the "Preview" button in Template Farm.
> If a preview frame is not set manually, then the last frame of the composition will be used.
```Syntax
#[Template Name]#preview(Frame1,Frame2...)
```
```Example
#MyTemplate#preview(50,100)
```

## Hide Template
Adding the "hide" tag to the name of the composition will not show it in Template Farm, but will still render it.
This is useful when you want to have multiple compositions controlled from a single template,
or you want to render the same clip with multiple resolutions. You can set a different composition for each resolution,
hide them and control the content with a visible composition that will not be rendered by using the [render(false)](Render Template) tag.
```Syntax
#[Template Name]#hide
```
```Example
#MyTemplate#hide
```

## Render Template
By default, all compositions that are marked as templates (names start with a #) are set to **render(true)**,
so there is no need to use this tag when the intention is to render the composition.
But when you want to use the composition only as template but not actually render it, set this tag to false as shown in the example.
```Syntax
#[Template Name]#render(true/false)
```
```Example
#MyTemplate#render(false)
```
Doing so, will show the composition as a template in Template Farm, but will not render the composition when the user clicks **Produce**.
For example you have a composition that is set to **render(false)** nested in a hidden composition.
This way the composition will show up in Template Farm, will be modified but will not be rendered as an individual composition,
just as a content of another composition.

## Required Template
Marking a template as **required** means that it will always be used. The user will no longer have the option to include or exclude this template from a production.
> By default, a template is not required and the users can choose if they want to include it in their production.
```Syntax
#[Template Name]#required
```
```Example
#MyTemplate#required
```

## Character Limit
Applying the "Limit" tag to a text layer will limit the amount of characters the text field will accept.
After the limit amount is reached the user will no longer be able to type in new characters.
In the following example the amount of characters is limit of 50.
```Syntax
#[Field Name]#limit(Number of allowed characters)
```
```Example
#MyField#limit(50)
```

## Rename Layer
Adding the rename tag to the layer name will rename the layer in the project view in Template Farm.
This is useful if you want to show multiple layers with the same name.
> If you name your layers with the same name in After Effects without using the "name" property, there will be naming conflicts in the composition and only the last property of the same-name properties group will be modified.
```Syntax
#[Field Name]#rename(New Name)
```
```Example
#MyTitle#rename(MyNewTitle)
```

You can also use the "nameFrom" tag to assign the name of another template to the current template.
This command is useful when the template is hidden but you still want to control the name of the hidden template.

For example, if you want to get the name from the template "#MySourceComp" the command should look like this:
`#MyTargetComp#nameFrom(MySourceComp)`

> Note that the hashtag is omitted from the source template name.
```Syntax
#[Template Name]#nameFrom(The name of the source comp)
```
```Example
#MyTemplate#nameFrom(AnotherTemplate)
```

## Dynamic Length
Adding the dynamicLength tag to the layer name will use this layers as an indication to the length of the parent composition.
When using the dynamicLength the composition duration will change according to the duration of the footage that is currently used for this layers content.
> This is useful when you want to load a video that will be overlayed with logos, lower thirds, etc.
```Syntax
#[Field Name]#dynamicLength
```
```Example
#MyTitle#dynamicLength
```

## Reorder Fields
Normally the fields of a template will be displayed in the order they are located in the composition.
Using the **Order** command will order the fields in an ascending or descending order alphabetically.
Use **asc** to indicate ascending order (A-Z) or **desc** to indicate descending order (Z-A).
> Using the tag without specifying the ordering direction (without "asc" or "desc") will assume ascending ordering by default.
```Syntax
#[Template Name]#order(direction)
```
```Examples
#MyTemaplte#order
#MyTemaplte#order(asc)
#MyTemaplte#order(desc)
```

If you are looking to create a custom ordered fields and the **order** tag is not enough, the **index** tag will assign an order value to each field
and will order the fields according to that value.
> Index value must be a whole, positive number.
> The "index" tag gets evaluated after the "order" tag and it is possible to use the two tags together.
For example: You can order the controls by name first and then assign indexes to the controls you want to order manually. In the example below, the second layer will be shown first because it has a lower index.
```Syntax
#[Field Name]#index(number)
```
```Examples
#MyTitle#index(1)
#MySubtitle#index(0)
```

## Lock Crop Aspect
When you choose an image in the image browser, you can choose to crop it and size it.
When the crop tool is on, another button will show that will allow you to lock the aspect of the crop.
The values of the aspect lock are determined by the size of the parent composition of the footage layer.
For example: if your footage layers is nested in the composition of 1000 x 1000, your aspect will be 1 x 1, so when you click on the lock aspect button, your crop gizmo will change to a square.

You can define the default state of the aspect by using "lockAspect" and defining it as **true** or **false**.
By default it's **false**. If it's set to **true**, the aspect of the crop will be locked when you click on the crop button, but you can always unlock it.

You can also choose to force your users to crop the image. In other words, you only allow images that are cropped.
In this case your users will not have the option to choose to crop or not to crop, or choose to lock the aspect. Instead the crop will always be on with the aspect locked, and the corresponding buttons will be hidden.
```Syntax
#[Field Name]#lockAspect([true / false / force])
```
```Examples
#MyTitle#lockAspect(true)
#MyTitle#lockAspect(false)
#MyTitle#lockAspect(force)
```

# Custom UI Elements
Every layer in the composition already has a default UI elements associated with it.
For example a text layer will be displayed as a text field in Template Farm, a footage layer will be displayed as a browse button and a solid layer will be displayed as a color picker dialog.
By using the **ui** tag you can override the default UI elements associated with each layer. The **ui** tag always starts with the same syntax, but accepts different parameters.

## Text Field
The layer will be shown as a text field in Template Farm. This is the default setting for any text layer.
 > This is useful when you want to pass a custom URL to a footage layer. Since footage layers are displayed as browse buttons by default.
```Syntax
#[Field Name]#ui(text)
```
```Example
#MyTitle#ui(text)
```

## Slider
The layer will be displayed as a slider in Template Farm.
In the example below, the slider minimum value is 0, maximum value 100 and the user can increase or decrease the value by 1.
The value of the slider will be applied to the layer value, so if the layer is a text layer, the value of the slider will be shown as plain text.
> This is useful when working with expression controls where the value is affecting something else in the composition.
```Syntax
#[Field Name]#ui(slider,[min],[max],[step])
```
```Example
#MyTitle#ui(slider,0,100,1)
```

## Dropdown
The layer will be displayed as a drop down box with options that were defined in the **ui** tag.
In the example below, the options will be **Red**, **Green** and **Blue**.
> This is useful when you want to limit the content that users can use for a field.
```Syntax
#[Field Name]#ui(options,[option1],[option2],[option3]...)
```
```Example
#MyTitle#ui(options,Red,Green,Blue)
```

You can also pull the contents for the drop down box from a folder in your project. This use a list of all the items in the folder and the selection options on the drop down.
This is useful if you would like to allow users to pick a specific footage item from a predefined list. Place all the desired footage items in a single folder and point to the folder by name.
In the example, the drop down list will display all the names of the footage items that are located in the project folder named "Icons".
```Syntax
#[Field Name]#ui(optionsFrom,[Project folder name])
```
```Example
#MyTitle#ui(optionsFrom,Icons)
```

## Map Files
> Avilable with Farmer version 1.6.1.5 and up
Map a local folder and its sub-folder to a dropdown selector. The dropdown will present a file browser where users can choose the file that will replace the footage content of the layer.
The map ui element will only include media files in the browse dropdown. Such as .jpg .png. .mov etc. This is useful when you work with large video files but do not want to upload them. 
> This functionality is available ONLY when the video files are accessible from the instance that the Farmer is installed on (for example a local folder or network).
```Syntax
#[Field Name]#ui(map,[Path to local folder])
```
```Example
#MyFootage#ui(map,C:\sample footage\videos)
```

# Layout
The following tags will allow you to control the way fields are presented and laid out in the template in Template Farm.

## Comments
Starting the name of a NULL layer with # will create a separator between the other controls in the project view of Template Farm.
The position of the separator is relative to the order of the layers in the After Effects project.
The title of the separator is the name of the layer.
In the example, the separator will be shown with a title "MySeparator". This is useful if you want to organize your controls in a more efficient order for your end users.
```Syntax
#[Comment]
```
```Example
#This is a separator with a comment for the end user
```

## Display Width
Using this tag will set the width of the control in the template (in percentage).
In the example below, the control for the layer with be set to be 50% of the total width of the controls column.
This tag allows the users to have multiple controls side by side by setting their width to less than 100% (the default value).
For example, setting the layers to be 33.33% percent wide, will place 3 controls side by side on the same row.
```Syntax
#[Field Name]#displayWidth([width value in percent])
```
```Example
#MyTitle#displayWidth(50)
```

## Hide Title
When using the **noTitle** tag, the control will show in the controls list without the title.
This is useful in combination with the **displayWidth** property, when creating a table where the title is always the same.
```Syntax
#[Field Name]#noTitle
```
```Example
#MyTitle#noTitle
```