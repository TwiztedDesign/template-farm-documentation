# Template Farm API
The Template Farm API allows external applications to interface with Template Farm. App can populate content into templates and produce new content
using their own UI with out the need for users to login into Template Farm. The API is still operating under the restrictions of the account it will authenticate to.
This means that the amount of content you account is allowed to produce will be the limit of what you can produce via the API.

# Authentication
In order to start working with the API you'll need to authenticate it with an API that can be found [here](https://www.templatefarm.io/account/settings).
To authenticate your request, append your API key in one of the following methods:
* As a header with the name: "x-api-key"
* As a query param with the name: "apikey"
* In the body as a param with the name: "apikey"

# Producing Content
## Getting the project
Once you have the API key, you'll need to get the project key. The project key is a unique identifier for every project in your account.
Open the project you want to interact with in Template Farm. You should see the project key as a part of its URL. It should like this:
`https://www.templatefarm.io/project/llTt86vbyVcQfoLRtNJ1kQLZMwtVIKR1s49k8-JTyV1hSjjtfSM7nBYSaNo3DBaSiWYPD-MnwDVToPPywr26GkD_1ETEsAPSaxCEHx099RCDgJTTQyj9vRJNw4Jt9xH82yfB4BlnrgHw0Rhxl1U2gqGH60U=`

The part after the `project/` is your project key.

## Modifying content
Once you get the project key, you can make calls to the API to render new content from that project via the API.
Make a POST request to the following url and replace {PROJECT KEY} with your project key:
`POST https://www.templatefarm.io/api/project/{PROJECT KEY}/produce`

You can pass the values you need to change in the project via an JSON object in the request body, like this:
`{
     "templates" : [
         {
             "name": "Title",
             "controls" : [
                 {
                     "name" : "text",
                     "value": "test title"
                 }
             ]
         }
     ]
 }`