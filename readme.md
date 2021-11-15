# AJAX Demo
![desired ouput](https://cdn.discordapp.com/attachments/899840196273639435/909612813125828638/ezgif.com-gif-maker_1.gif)

Here is a list of steps we are going through this demo:
- [AJAX Demo](#ajax-demo)
- [Definitions](#definitions)
- [Why AJAX](#why-ajax)
- [AJAX ✕ jQuery](#ajax---jquery)
- [Part I - HTML](#part-i---html)
- [Part II - JavaScript](#part-ii---javascript)
- [Part III - JSON](#part-iii---json)
- [Debugging Tips](#debugging-tips)
- [See also](#see-also)
- [Check Your Understanding](#check-your-understanding)


---
<!-- Check out the complete server code [here](./server/server.js) and client code [here](./client/). -->
<!-- --- -->
# Definitions
- AJAX: Stands for Asynchronous JavaScript and XML. We will use jQuery instead of JavaScript and JSON instead of XML.
- JSON: JSON is a way of representing data that corresponds to the syntax of JavaScript objects. It allows us to send/receive objects easily over the Internet.

- jQuery: The most popular JavaScript Library. Provide functions to modify/retrieve HTML, Add effects to the page, and format AJAX request!
- HTML: Stands for HyperText Markup Language and it controls the layout of the static elements on the web page.
- API: Stands for Application Programming Interface. In this tutorial, we need an API to communicate with a web service to get the weather data for free!

---
# Why AJAX
- There is a clear distinction between what is displayed on the page (content), appearance (style), and page functionality (feature).

- Programmers who change what they need for a page don't have to worry about what the page looks like.

- A pages created with Ajax load only small bits from the server. In most cases, you will run the code on the client. This will load the program first and allow it to react faster.

- Ajax can be used specifically for reading XML or JSON.

---
# AJAX ✕ jQuery
AJAX is a technology that exchanges data with a server and updates parts of a website without reloading the entire page.
jQuery provides several methods for AJAX functionality. In this tutorial, we will demonstrate the `$.json()` function to perform an async AJAX request.

Let us start with this HTML skeleton code.

---
# Part I - HTML
```HTML
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="code.js"></script>
    <title>Weather App</title>
  </head>
  <body>

    Vancouver temperature is ..


  </body>
</html>
```

So here we are trying to display Vancouver's weather. Instead of hard coding the temperature in the HTML code. Let us retrieve it from the [*openweathermap*](https://openweathermap.org/) web service.

First things first. Let us start with writing a placeholder for Vancouver weather. Or even better, let us add a text field to get the weather of any city in the world!

Here is modified HTML code
```HTML
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="code.js"></script>
    <title>Weather App</title>
  </head>

  <body>
    Please enter city name:
    Enter City:<input type="text" id="city_name_input" value=""><br>

    <input type="submit" id="get_temperature_button" value="Get">
    Temperature is <span id="city_temperature"></span> celsius.

  </body>
</html>
```
Here is how would displayed by the browser:
![screenshot](https://cdn.discordapp.com/attachments/899840196273639435/909613851073478687/unknown.png)

Next, let us write a skeleton JS code to handle the click event on the button.


---
# Part II - JavaScript
```js
function ajax_get(){
  $.ajax(
    {
      //to do
    }
  )
}

function setup(){
  $('#get_temperature_button').click(ajax_get);
}
$(document).ready(setup);

```
Here is the `ready` function will call the `setup` function when the document is ready. The `setup` function will call the `ajax_get` function when the button is clicked by the user. Finally, the `ajax_get` function will make THE AJAX request.   

Now, let us fill the `$.ajax()` function. First, Notice that this function takes one argument of type JSON object. The JSON object is a set of key-value pairs. Here is our object:
```JSON
{
  'url':'https://api.openweathermap.org/data/2.5/weather?q=Vancouver&appid=b660f3402c54cb9a9c48f89c35249e5c&unit=metric',
  'type': 'GET',
  'success': procces_
}
```
The `url` key is the URL that we need to request the data from. The `type` key is the HTTP request's type. IT could be GET, POST, DELETE, or UPDATE. These requests correspond to the CRUD operations in Databases.

Anyhow, we need to focus only on the GET since we want to retrieve the weather of a specific city. The `success` field will take a function handle to process the return JSON object. It just happens that this API specfically returns a JSON obejct. Check their [docs](https://openweathermap.org/current#current_JSON)!

Alright, let us plug this JSON object as a parameter to the `$.ajax()` method:


```js
function AJAX_GET(){
  $.ajax(
    {  
      url:'https://api.openweathermap.org/data/2.5/weather?q=Vancouver&appid=b660f3402c54cb9a9c48f89c35249e5c&unit=metric',
      type:'GET',
      success: procces_
    }
  )
}

function setup(){
  $('#get_temperature_button').click(AJAX_GET);
}
$(document).ready(setup);

```

Next, let us implement the `process_` function:
```JS
function procces_(data){
  jQuery('#city_temperature').html(data.main.temp)
}
```
where `city_temperature` is the id of the placeholder for the returned temperature.

Now if you have noticed `data.main.temp`, then you have just caught me red handed. Where did it come from? Here where we have to explain the structure of returned JSON object from *openweathermap.org* web service.

Based on [their API documentation](https://openweathermap.org/current#current_JSON), here is the format of the returned JSON object:
```JSON
{
  "coord": {
    "lon": -122.08,
    "lat": 37.39
  },
  "weather": [
    {
      "id": 800,
      "main": "Clear",
      "description": "clear sky",
      "icon": "01d"
    }
  ],
  "base": "stations",
  "main": {
    "temp": 282.55,
    "feels_like": 281.86,
    "temp_min": 280.37,
    "temp_max": 284.26,
    "pressure": 1023,
    "humidity": 100
  },
  "visibility": 16093,
  "wind": {
    "speed": 1.5,
    "deg": 350
  },
  "clouds": {
    "all": 1
  },
  "dt": 1560350645,
  "sys": {
    "type": 1,
    "id": 5122,
    "message": 0.0139,
    "country": "US",
    "sunrise": 1560343627,
    "sunset": 1560396563
  },
  "timezone": -25200,
  "id": 420006353,
  "name": "Mountain View",
  "cod": 200
  }                         
  ```

  ---
  # Part III - JSON
  The previous listing is just one JSON object that has pairs of keys and values. One of the keys is the `main` key which its value happens to be another JSON object that contains a `temp` key. This key represents the current temperature of a city along with other key  information of other aspects of the city weather like visibility, wind, pressure, ..etc. We are only interested in extracting the temperature from the whole object.


  Another modification we need to make is to set the city in the `url` field in the `.ajax()` function to embed a choosen city from the user's input and not to have it hard coded in the JS code.

  Here is how to fix it using jQuery:
  ```js
  function procces_(data){
    jQuery('#city_temperature').html(data.main.temp)
  }

  function AJAX_GET(){
    city_name_input = jQuery('#city_name_input').val()
    $.ajax(
      {
        {
          url:`https://api.openweathermap.org/data/2.5/weather?q=${city_name_input}&appid=b660f3402c54cb9a9c48f89c35249e5c&unit=metric`,
          type:'GET',
          success: procces_

        }
      }
    )
  }

  function setup(){
    $('#get_temperature_button').click(AJAX_GET);
  }
  $(document).ready(setup);

  ```
Phew!

The final output:

![output](https://cdn.discordapp.com/attachments/899840196273639435/909612813125828638/ezgif.com-gif-maker_1.gif)


  ---
# Debugging Tips
- Use chrome developer tools, `console.log()`, and `alert()` JS functions
- Check the networking tab in the developer tools to see the outgoing requests.

# See also
- Check my other Demo on [MERN](https://github.com/nabil828/mern_demo) stack. It might help you to write a POST AJAX request to modify a database on the server side.


# Check Your Understanding
In AJAX you can:

A)Update a web page without reloading the page

B)Receive data from a server - after the page has loaded

C)Send data to a server - in the background

D) All of the above
<details><summary>Answer</summary>

All the above!

</details>



Thanks,
<br/>
Nabil M. Al-Rousan
