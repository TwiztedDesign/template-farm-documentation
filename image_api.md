# API API
An API to convert images and manipulate SVGs. You can make a GET request to https://www.templatefarm.io/api/image/**FARM_ID**/YOUR_PATH with the params detailed below. For a GET request pass the params as query params in the request URL.

## Authentication
For all GET requests the first pram in the request should be the [Farm ID](https://www.templatefarm.io/account/settings/farm).
```Example
https://www.templatefarm.io/api/image/FARM_ID/test.svg?output=jpeg&quality=50&height=200&props=[{"name":"input.settings.tags.snowboard","visible":false}]
```


## Request Structure
The following query parameters can be passed with any request:

| Param | Value | Description |
| ----- | ----- | ----------- |
| height| Positive Integer | Set the height of the returned image |
| width| Positive Integer | Set the width of the returned image |
| size | xs, sm, md, lg, xl | Overrides the width and height settings with a predefined size |
| quality | Integer 0-100 | Sets the quality of the returned image (not applicable to SVG unless converted to another format) |
| output | jpeg, png, webp | Converts the images to the specified format |
| props | Array of Objects | An array of properties to modify (Only for SVG) |

```Example
https://www.templatefarm.io/api/image/FARM_ID/test.svg?output=jpeg&quality=50&height=200
```
In the example above we convert the original image to jpeg, drop the quality to 50%, and set the height to be 200 pixels. Since there was no width set explicitly it will be adjusted automatically in relation to the height.

## Props Structure
When the props param is passed to the request, the API will try to manipulate the specified SVG using the props array. When passing the props param make sure to strongly type the JSON structure. Each object within the props array has the following structure:

| Param | Value | Description |
| ----- | ----- | ----------- |
| name | The ID of the SVG object to manipulate | This property is required. An attribute of "id" must be present in the SVG object |
| visible | Boolean | Set the visibility of the object |
| value | String / Number | Set the value of an object. For each object type in SVG. The value applied differently for each object type in the SVG |
| attr | Object | Set arbitrary attribute of the SVG object | 

```Example
https://www.templatefarm.io/api/image/FARM_ID/test.svg?props=[{"name":"property_1","visible":false, "name":"property_2": "value":"New Title"}]
```
In the example above we hide the element that has an id of "property_1", and we set the value of the test layer with the id "property_2" to be "New Title".

### Value application per object type
When a value is set on an SVG object will will apply differently based on the type of the object:

| Type | Result |
| ---- | ------ |
| image| Set the path to the image source (xlink:href) | 
| text | Set the text string value of the object | 
| g    | Select a single child from the group and hide all others. If the value is a number - the selection is by index (zero-based). If the value is a string - the selection is by the "id" attribute (similar to the "name" param) |

### Arbitrary Attributes
When settings arbitrary attributes with the "attr" param, the expected value is an object containing a key-value pair. The key is the attribute name and the value is the attribute value. for example: **{fill : "#ffffff"}**