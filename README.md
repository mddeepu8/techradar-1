[![Build Status](https://travis-ci.org/soneymathew/techradar.svg?branch=master)](https://travis-ci.org/soneymathew/techradar)

A library that generates an interactive radar that is tailored to satisfy the need of keeping the radar data private, inspired by [thoughtworks.com/radar](http://thoughtworks.com/radar).

## Demo

If you plug in [this data](https://docs.google.com/spreadsheets/d/1YXkrgV7Y6zShiPeyw4Y5_19QOfu5I6CyH5sGnbkEyiI/) you'll see [this visualization](https://atlassian.github.io/techradar/?sheetId=1YXkrgV7Y6zShiPeyw4Y5_19QOfu5I6CyH5sGnbkEyiI). 

## How To Use

There are a few ways to provide data to the radar

#### Published google spreadsheet
The easiest way to use the app out of the box is to provide a *public* Google Sheet ID from which all the data will be fetched. You can enter that ID into the input field on the first page of the application, and your radar will be generated. The data must conform to the format below for the radar to be generated correctly.

#### google doc `sheetId` via url
You can set the sheetId query parameter to render radar at launch
* Example : https://atlassian.github.io/techradar/?sheetId=1waDG0_W3-yNiAaUfxcZhTKvl7AUCgXwQw8mdPjCz86U

#### JSON via url
You can set the dataUrl query parameter to render radar at launch and use json data served from a url
* Example : https://atlassian.github.io/techradar/?dataUrl=https://api.myjson.com/bins/kw6y9

#### JSON via url specified in a global variable
* If you have set the variable window.LOCAL_DATA_URL radar will use that to render data at launch
This is convenient if you don't want to clutter the url with the dataurl when you publish the radar

#### JSON via data specified in a global variable
If you have set the variable window.LOCAL_DATA radar will use that object to render data at launch

### Setting up your data as JSON
The structure of the object should be as mentioned in `index.html`
The Json returned should be following the below schema
```javascript
      {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "name": {
              "type": "string"
            },
            "ring": {
              "type": "string"
            },
            "quadrant": {
              "type": "string"
            },
            "isnew": {
              "type": "boolean"
            },
            "description": {
              "type": "string"
            }
          },
          "required": [
            "name",
            "ring",
            "quadrant",
            "isnew",
            "description"
          ]
        }
      } 
```

### Setting up your data in spreadsheet

You need to make your data public in a form we can digest.

Create a Google Sheet. Give it at least the below column headers, and put in the content that you want:

| name          | ring   | quadrant               | isNew | description                                             |
|---------------|--------|------------------------|-------|---------------------------------------------------------|
| Composer      | adopt  | tools                  | TRUE  | Although the idea of dependency management ...          |
| Canary builds | trial  | techniques             | FALSE | Many projects have external code dependencies ...       |
| Apache Kylin  | assess | platforms              | TRUE  | Apache Kylin is an open source analytics solution ...   |
| JSF           | hold   | languages & frameworks | FALSE | We continue to see teams run into trouble using JSF ... |

### Sharing the sheet

* In Google sheets, go to 'File', choose 'Publish to the web...' and then click 'Publish'.
* Close the 'Publish to the web' dialog.
* Copy the URL of your editable sheet from the browser (Don't worry, this does not share the editable version). 

The URL will be similar to [https://docs.google.com/spreadsheets/d/1waDG0_W3-yNiAaUfxcZhTKvl7AUCgXwQw8mdPjCz86U/edit](https://docs.google.com/spreadsheets/d/1waDG0_W3-yNiAaUfxcZhTKvl7AUCgXwQw8mdPjCz86U/edit). In theory we are only interested in the part between '/d/' and '/edit' but you can use the whole URL if you want.

### Building the radar

Paste the URL in the input field on the home page.

That's it!

Note: the quadrants of the radar, and the order of the rings inside the radar will be drawn in the order they appear in your Google Sheet.

### More complex usage

To create the data representation, you can use the Google Sheet [factory](/src/util/factory.js), or you can also insert all your data straight into the code.

The app uses [Tabletop.js](https://github.com/jsoma/tabletop) to fetch the data from a Google Sheet, so refer to their documentation for more advanced interaction.  The input from the Google Sheet is sanitized by whitelisting HTML tags with [sanitize-html](https://github.com/punkave/sanitize-html).

The application uses [webpack](https://webpack.github.io/) to package dependencies and minify all .js and .scss files.

## Contribute

All tasks are defined in `package.json`.

Pull requests are welcome; please write tests whenever possible. 
Make sure you have nodejs installed.

- `git clone git@github.com:soneymathew/techradar.git`
- `npm install`
- `npm test` - to run your tests
- `npm run dev` - to run application in localhost:8080. This will watch the .js and .css files and rebuild on file changes

### Don't want to install node? Run with one line docker

     $ docker run -p 8080:8080 -v $PWD:/app -w /app -it node:7.3.0 /bin/sh -c 'npm install && npm run dev'

After building it will start on localhost:8080
