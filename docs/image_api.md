# Template Engine Image API 
The Image API allows you to generate images dynamically by sending parameters to Template Engine cloud templates. To generate a new image, make a GET request to https://imageapi.templatefarm.io/**FARM_ID**/**TEMPLATE_ID**/image with query params detailed below. For a GET request pass the params as query params in the request URL.

For example:
```
https://imageapi.templatefarm.io/FARM_ID/TEMPLATE_ID/image?scoreA=5&scoreB=7
```
The above example generate and image from a template and updates the layer called **scoreA** with the value of **5**, and the value of the layer **scoreB** with **7**.

You can also call the same image with a post request and pass the values in the body of the request, instead of the query params. This is useful in cases the url get too long to to many params in the template need changing.
Using the example about but sending it as a POST request:

Send the request to the same URL, but omit the query params:
```
https://imageapi.templatefarm.io/FARM_ID/TEMPLATE_ID/image
```
Instead of adding the query params, send the params as a **body** payload:

```
{
    "scoreA": "5",
    "scoreB": "7"
}
```

> While the GET request is open, the POST request requires additional authentication. Add a header of **x-api-key** with your API key. The API key can be obtained from your Template Engine account settings.

> When your template contains many params, your request url might get very long. Some browsers have a url length limit. In those cases consider using the post request and pass the parameters in the **body** of the request.

## Request Structure
When the query params are passed with the request, the API will try to manipulate the template using the params. Each param is a key and value;
The key can be the layer name or the layer name and a specific property. 

For example:
| Prop | Description |
|------|-------------|
|right_score=5  |  Modify the **text** value of the layer "right_score".|
|right_score.opacity=0.5 | Set the opacity of the layer "right_score" to 50%. |

#### Wildcards
In order to apply the same value to multiple layers you can use the wildcard operator in the name (*).


For example:
| Prop | Description |
|------|-------------|
|*score=5 | Modify the **text** value of all layers with the name ending with "score".|
|logo*.visible=false | Hide all layers with the name starting with "logo" |

When a layer property is not passed in the query param, the following defaults will apply:
| Layer Type | Property Type| Default Property |
|------------|--------------|------------------|
| Text | String | Text value |
| Image | String | Source of the image |
| Shape | HEX String | Fill color |

The layer properties that are currently supported are:

| Prop | Type | Description |
| ----- | ----- | ---- |
| opacity | Float | Control the opacity of the layer |
| fill | HEX Color | Set the visibility of the object |
| visible | Boolean | Set the visibility of the layer |
| stroke | Hex Color | Set the stroke color of the layer | 
| strokeWidth | Float | Set the stroke width of the layer | 


### Custom Properties
It is possible to pass additional properties to are converted to values automatically. This enables you to create conditional templates that respond uniquely per viewer. The custom properties always start with a double underscore, just like any other special params.

| Property Name | Property Type
|------------|--------------|
| __isMobile | Boolean |
| __isDesktop | Boolean |

In the following example we show the show the layer called **layer_1** only if the requesting device is a mobile device.

```
https://imageapi.templatefarm.io/FARM_ID/TEMPLATE_ID/image?layer_1.visible=__isMobile
```

### Working with assets
To pass a param that populates an image layer, you need to pass the asset name as it appears in the editor in the assetsYou can use one of the following options:

#### Loading images from the assets library
To load an asset from the assets library you can pass the asset name or the asset path as it appears in the assets library.

> If only the name of the asset is passed, like so **logo=asset1**, the API will try to find the first asset matching the name **asset1** in the entire asset library (including all sub folders)

> If a path is passed like so **logo=logos/asset1**, the API will try to find the first asset matching the name **asset1** in the folder **logos** in the asset library.

#### Loading images from the web
To load an asset from the web, simple pass the full url os the image. the url string must start with **https**.

### Special params
The API supports an additional set or params that allows you to control properties beyond layer manipulation.
Such params will always start with a double underscore:

| Prop | Type | Description | Default (if omitted) |
| ----- | ----- | ---- | ---- |
| __output | String | Set the output file format. Options are 'jpeg', 'png', 'webp' | 'jpeg' |
| __size | Float | Set the size of the returned image in percent. For example a value of 0.5 | 1 (Full size)| will return 50% of the original template size |
| __page | Integer | Set the page to be used when rendering the template. If not specified, the first page of the template will be returned | 0 (First page)
| __data | String | The compressed base64 string of the query prarms |

### URL Compression
When there is a lot of data to be passed as query prarms it is possible to compress the query prarms using the build it LZMA compression algorithm.

to use compressed data simply pass it as a **__data** query param, so the complete urls will look something like this:

```
https://imageapi.templatefarm.io/FARM_ID/TEMPLATE_ID/image?__data=XQAAgAAUAwAAAAAAAAAv4cjSfdValpeiKCiRIuTcLLA5uzyPH_a11f9LlWYK6nIMBLZSiGzEpxMlkDdxf4e2VWneQ1TUlOcxXIh-f1ORdXnXNRYN75XnTjKaCXPwUJT2GSe-oBtN8oNcJdTTWl1yRcsdkFksHnCKd...
```

#### Temporary compression endpoint
To compress the params, send a POST call to this temporary endpoint:
```
https://imageapi.templatefarm.io/FARM_ID/TEMPLATE_ID/compress
```
add all params to the **body** same as with the normal POST request. This endpoint will return a JSON object that looks like that:
```
{
    "originalLength": 342,
    "compressedLength": 184,
    "compressionRatio": 0.4619883040935673,
    "url": "https://imageapi.templatefarm.io/FARM_ID/TEMPLATE_ID/image?__data=XQAAgABWAQAAAAAAAAA6Gkn64VeqvzgPlW-mDu6rzGPGF6I4YyEcWNuiupTOficGZL8k1hR0sK6jCRHMjvW7miDdfM0pkiRJMpFvXYPBR11sTcc-N0tcvOF0esBpXWJYK5vRAhIaX1awiq4RrkokJVDWREtefDcVoy2zUpIxKv__tZJgAA%3D%3D"
}
```
> Note that compressing small amounts of data will result in a reverse effect and will inflate the payload. Use with caution!

> Moving forward the compression will need to be done on the side on the code implementation of the integration to the Image API. You can copy the following files from the repo
1. ./services/base64.js
2. ./services/lzma.js

And then implement compression like so:
```
const lzma = require('./lzma.js');

// Generate the query params from your data source
const params = 'prop1=Hello&prop2=World';

// Compress the params
const compressed_data = lzma.compressUrlSafe(params);

// pass the compressed_data in the regular request url as the value of __data property.

return 'https://imageapi.templatefarm.io/FARM_ID/TEMPLATE_ID/image?__data=' + compressed_data;
```
