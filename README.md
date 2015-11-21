```
 ____                         __        __         _   _               _ 
/ ___| _   _ _ __   ___ _ __  \ \      / /__  __ _| |_| |__   ___ _ __| |
\___ \| | | | '_ \ / _ \ '__|  \ \ /\ / / _ \/ _` | __| '_ \ / _ \ '__| |
 ___) | |_| | |_) |  __/ |      \ V  V /  __/ (_| | |_| | | |  __/ |  |_|
|____/ \__,_| .__/ \___|_|       \_/\_/ \___|\__,_|\__|_| |_|\___|_|  (_)
            |_|                                                          
```
# Super Weather App Workshop

## Presented by DecodeMTL :)
[![DecodeMTL](http://www.decodemtl.com/assets/images/decodemtl-logo-black.png)](http://www.decodemtl.com)

In this workshop, we will start with a basic one-page application and make it better in a number of ways.

During the first hour, we will be reviewing together what is currently available, as well as discussing the various points that can be worked on to make the app better.

This single-page application is built using [jQuery](https://jquery.com), [Underscore](http://underscorejs.org) and [Backbone](http://backbonejs.org/).

jQuery allows us to [easily manipulate the DOM](https://api.jquery.com/category/miscellaneous/dom-element-methods/) of our page to display new data, as well as do the [fetching of data using jQuery.getJSON](https://api.jquery.com/jquery.getjson/) which is basically an AJAX request.

[Underscore allows us to create HTML templates](http://braddunbar.net/2013/08/19/underscore-templates/). They are basically re-usable pieces of HTML code, interspersed with so-called template tags, where we can output values to "fill in the blanks". For example a template like `<h1>Hello <%= data.name %></h1>` contains a template tag that will output the value of `data.name`.

Backbone allows us to separate the concerns of our code between the data fetching and the presentation.

Using [Backbone's Models](http://backbonejs.org/#Model), we are creating structures that can efficiently fetch data and update themselves. On every update, these data structures will emit a `change` event. We can subscribe to this event -- no different from a user mouse click event really -- and do things, by using [Backbone's Events system](http://backbonejs.org/#Events-on). E.g. if we want to get notified that the user's location has changed, we can do `locationModel.on('change', myFunction)` where `myFunction` is how the model will notify us of the change.

Finally, using [Backbone's View system](http://backbonejs.org/#View), we are creating a structure that can render itself to the DOM of the webpage. The view uses the Underscore template we defined, as well as the model's change event to render the current weather on the page. Backbone Views have so much more to offer than simply rendering themselves: they can efficiently bind/unbind from/to DOM events, and make it easier to encapsulate functionality inside one particular DOM element.

Pretty much every activity below can be tackled independently, and will make you learn about a new aspect of developing web applications.

**This means the activities do not have to be done in any order!** Feel free to choose what you prefer working on, and we will do our best to help you with it.

**IMPORTANT**: Before getting started, register for an API key on [forecast.io](https://developer.forecast.io/) and change the line of the code that says `var API_KEY = ...` to your own key. If we all use the same key, we will get rate limited and everyone will have a bad time :(

## Activity 1: Making it look good...
In this activity, you will be using CSS to create a nice looking UI for the weather app. Here are a few indications to get you started:

* Here is an example UI from weather underground:

![wunderground](https://i.imgur.com/xN9iGxo.png)
* Here is a more ambitious UI from weather network:

![weather network](https://i.imgur.com/l52mgdW.png)
* You should start small, implementing only a few elements at first, then adding more data
* To display nice looking weather icons, take a look at [forecast font](http://forecastfont.iconvau.lt/). Add it to your page, and look at the forecast.io API results for the `icon` property.
* Modern layouts are created using CSS' Flexbox system. [Learn more about Flexbox](https://cvan.io/flexboxin5/) with this interactive tutorial. It will help you create a nice UI with little effort.
* Read up on the [vw and vh CSS units](https://css-tricks.com/viewport-sized-typography/). They will help you create a UI that scales well to display the weather on different screen sizes.
* You will have to modify the Underscore template in the `app.html` code to add new elements to the markup, whether they be classes or new HTML elements.
* Experiment. CSS is all about trying things. Make sure to use Chrome's Developer Tools to get quick feedback about a change you are trying to make.

## Activity #2: Adding the location city
In this activity, we are going to improve the UI by adding the city name for which we are fetching the weather.

Unfortunately, the HTML5 geolocation API only returns a latitude/longitude, and forecast.io's API doesn't return any location names with its weather data.

We would like to display, e.g. "Weather for Montreal, QC" or something meaningful. Here are some indications to help you:

* Google has a Geocoding API (takes a place name and gives a latitude/longitude). One of its features is [Reverse Geocoding](https://developers.google.com/maps/documentation/geocoding/intro#ReverseGeocoding) which takes a latitude/longitude, and returns a series of places that match it.

* [Get an API key](https://developers.google.com/maps/documentation/javascript/get-api-key) from Google to enable you to use Reverse Geocoding.

* [Look at an example of results](https://maps.googleapis.com/maps/api/geocode/json?latlng=45.5,-73.5&key=AIzaSyCqB6bDHnvEmYxcRm5qg6yso2V9Q4MbOiE). The URL for this one is https://maps.googleapis.com/maps/api/geocode/json?latlng=45.5,-73.5&key=AIzaSyCqB6bDHnvEmYxcRm5qg6yso2V9Q4MbOiE but you should always be adding your own `&key` by getting an API key from Google!

* Each result has a `types` property. Look for the one where the `types` contains `locality` and `political`, it is the friendliest name. The name is available as `formatted_address`.

* If you are having trouble viewing the JSON result, check out the [JSONView extension for Chrome](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en). It will format the JSON in a user-friendly way.

* Create a new Backbone model called `CityModel` similar to the `WeatherModel`. It will use `LocationModel` to find out the current lat/long, and make a query to Google Geocoding. When you get back the answer, set your `city` attribute to be what you found out from Google.

* Create a new Backbone view that will subscribe to the change in a `CityModel`. Its template will use the `city` value from the model to display that to the user, above the weather data. Perhaps in an `<h1>` tag, but this would go beyond the goal of this exercise

## Activity #3: Adding a search bar at the top
**NOTE**: This is a more advanced activity!

In this activity, you will add a search bar at the top of the app. In this search bar, the user will be able to search for a city with an auto-completer. When choosing a suggested city, they should be shown the weather for that city instead of their current location.

Here are some indications for completing this exercise:

* You would be using [Google's Places Autocomplete API](https://developers.google.com/places/web-service/autocomplete) to get a suggestion box.
* You may need to read the [documentation on how to restrict the types of places](https://developers.google.com/places/web-service/autocomplete#place_types) you are being suggested.
* Currently, the `WeatherModel` and potentially the `CityModel` from the previous exercise have separate instances of `LocationModel`. Moreover, they are only getting your current location. You'll have to find a way to have a single `LocationModel`, and to enable it to have a custom location.
* When the user chooses a city from the suggestion box, you will have access to the latitude/longitude. Use those to modify the single `LocationModel` you have created.
* The `WeatherModel` and potentially `CityModel` from the previous exercise should subscribe to change events from the single `LocationModel` object, and update their display appropriately.

## Activity #4: Modularizing the code
**NOTE**: This is a more advanced activity!

During the workshop, we talked a bit about MVC. Separating concerns between data fetching and display is great, but we can go one step further and split up our code to make it more maintainable.

We will be using the [NPM](https://www.npmjs.com/) package manager to manage the dependencies of our app (jQuery, Underscore and Backbone).

We will also be using [Webpack](https://webpack.github.io/) to bundle up all our application into one `app-bundle.js` file, and to allow us to create our own internal modules. For example, we'll see tha the Underscore template can go in its own file, removing the need for a weird `<script type="text/template">` tag. Each model and view should also be able to be separated from the rest of the code.

Here are some indications to get you started:

* As a first step, take all the JavaScript on the page, extract it to an `app.js` file, and link to it from the HTML.
* Read up about [CommonJS Modules](http://spinejs.com/docs/commonjs). These are the types of modules we will be creating.
* Here are a few hints to create your first module, the `LocationModel`:
  * Create a directory called `lib` at the root of your project.
  * Create a new file at `lib/location-model.js`.
  * Put the code of `LocationModel` in there, and add a `module.exports = LocationModel`.
  * Try to `require` this module from your `app.js`
  * At this point your browser will complain that the `require` function does not exist. This is normal. We are going to use Webpack to resolve all the dependencies starting in our entry point, `app.js` and going down the tree. Then Webpack will build up an `app-bundle.js` that will contain all our code in the correct order.
  * Read [Webpack's Getting Started Guide](https://webpack.github.io/docs/tutorials/getting-started/) and follow its activity separately, until you are comfortable compiling a simple `app-bundle.js`
  * Compile your own `app.js`, and modify the HTML file to point to `app-bundle.js`
* Based on the previous hints, try to modularize as much of your code as possible, controlling everything thru `app.js` and re-compililng with Webpack every time you make a change.
* Once you get tired of re-compiling every time you make a change, the getting started guide mentions a "watch mode": this will make webpack watch for changes in your files, and recompile every time you save a file.
* As a final step, we can take care of the external dependencies of our project (jQuery, Underscore and Backbone) and bundle them up with our app. Instead of the three `<script>` tags at the top of our HTML, we will use NPM to define our dependencies and Webpack will automatically find them:
  * Run `npm init` to create a manifest for your web app. This will ask you a few questions and create a file called `package.json`. Among other things, it will list our dependencies.
  * Run `npm install --save jquery@1.11.3 underscore backbone`
  * This will install everything we need, but also put it in `package.json` for future reference.
  * Remove the 3 `<script>` tags from the HTML page
  * At this point your application will break again, because the global variables `$`, `jQuery`, `_` and `Backbone` are not available anymore.
  * Revisit every module you wrote, and make sure to `require` the dependencies it is using. For example, your `LocationModel` depends on `Backbone` and `$` (jQuery) so you would add the following to the top of the module:
  ```javascript
  var $ = require('jquery');
  var Backbone = require('backbone');
  ```
