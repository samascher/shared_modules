# <img src="https://cloud.githubusercontent.com/assets/7833470/10899314/63829980-8188-11e5-8cdd-4ded5bcb6e36.png" height="60"> Week 2 Assessment [SOLUTIONS]

### 1. DOM Manipulation

Use this HTML for the following questions:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My Site</title>
</head>
<body>
  <nav>
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
      <li><a href="/contact">Contact</a></li>
    </ul>
  </nav>
  <div class="section">
    <h1>Welcome to my site!</h1>
    <img src="cheese.jpg">
    <p>Words and <a href="/">links</a></p>
  </div>
  <div class="footer">
    <small>Copyright 2015 WDI 24</small>
  </div>
</body>
</html>
```

**Using jQuery...**

1.1 Replace "Welcome to my site!" with "Hello World!"

  ```js
  $('h1').text('Hello World!');
  ```

1.2 Replace the current image with this image: "earth.jpg"

  ```js
  $('div.section').html('<h1>Welcome to my site!</h1><img src="cheese.jpg"><p>Words and <a href="/">links</a></p>');
  //or a better way that you haven't seen yet
  $('img').attr('src', 'earth.jpg');
  ```

1.3 Add a new link to the nav that points to the "/blog" section of the website.

  ```js
  $('nav ul').append('<li><a href="/blog">Blog</a></li>');

  // less specific, still works for this example
  $('ul').append('<li><a href="/blog">Blog</a></li>');
  ```

1.4 When a user clicks on one of the `nav` links, do not redirect them. Instead, trigger an `alert` that says "Sorry, this page is under construction". **Bonus:** Display the page name ("Home", "About", etc.) in the alert.

  ```js
  $('nav a').on('click', function(event) {
    event.preventDefault();
    alert("Sorry, this page is under construction");
  });

  // other ways of selecting nav links
  $('nav ul a').on(...)
  $('nav > ul a').on(...)
  $('ul a').on(...)
  $('nav ul li a').on(...)
  $('nav > ul > li > a').on(...)
  $('ul li a').on(...)
  $('ul > li > a').on(...)

  // bonus
  $('nav a').on('click', function(event) {
    event.preventDefault();
    var pageName = $(this).text();
    alert("Sorry, the " + pageName + " page is under construction");
  });
  ```

### 2. JavaScript OOP

2.1 Create a JavaScript object constructor for a `Person`. Every instance of `Person` should have the following properties:
  * first name
  * last name
  * number of siblings
  * states lived in

  ```js
  function Person(firstName, lastName, numSiblings, statesLived) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.numSiblings = numSiblings;
    this.statesLived = statesLived;
  }
  ```

2.2 Using your `Person` constructor, create a `greet` method that returns a string, e.g. "Hi, my name is Steve Wozniak and I have 3 siblings!"

  ```js
  function Person(firstName, lastName, numSiblings, statesLived) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.numSiblings = numSiblings;
    this.statesLived = statesLived;
    this.greet = function() {
      return "Hi, my name is " + this.firstName + " " + this.lastName + " and I have " + this.numSiblings + " siblings!";
    };
  }
  ```

2.3 Create a new instance of `Person`, and demonstrate how you would call the `greet` method.

  ```js
  var steve = new Person("Steve", "Wozniak", 3, ["CA"]);
  ```

2.4 Create a method called `placesLived` that prints all the states a person has lived in, one per line.

  ```js
  function Person(firstName, lastName, numSiblings, statesLived) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.numSiblings = numSiblings;
    this.statesLived = statesLived;
    this.greet = function() {
      return "Hi, my name is " + this.firstName + " " + this.lastName + " and I have " + this.numSiblings + " siblings!";
    };
    this.placedLived = function() {
      this.states.forEach(function(state) {
        console.log(state);
      });
    };
  };
  ```

### 3. AJAX

3.1 Name and describe the parts of the following URL: https://www.youtube.com/watch?v=y8Kyi0WNg40&t=1s

  ```
  https://www.youtube.com/watch: webpage URL
  ?: starts parameters
  v: parameter name/key
  y8Kyi0WNg40: parameter value
  &: separates parameters
  t: parameter name/key
  1s: parameter value

  The webpage URL is the base URL the client requests,
  which triggers some action on the server.
  The URL parameters send data to the server.
  ```

3.2 Juggler Supply Co. has an api with the following documentation:

  **API Endpoint:** http://jugglersupply.co

  **Search:** Returns all matching juggling supply products.
    * Path: `/api/supplies/search`
    * Parameters:
      * `q` - search query term or phrase.
      * `limit` - (optional) number of results to return. Default 10.
      * `offset` - (optional) results offset. Default 0.
      * `danger` - (optional) limit results to supplies with this danger level (`safe`, `medium`, `superdanger`).
    * Example response:
      ```js
      { "data": [
        { "name": "Simple Balls", "danger": "medium"},
        { "name": "Deceptively Simple Balls", "danger": "superdanger"}
      ] }
      ```

  **Random:** Returns a random juggling supply product.
    * Path: `/api/supplies/random`
    * Example response:
      ```js
      { "data": { "name": "Bunnies", "danger": "medium" } }
      ```

  **Top:** Returns the most popular juggling supply products.
    * Path: `/api/supplies/top`
    * Parameters:
      * `limit` - (optional) number of results to return. Default 2.
      * `danger` - (optional) limit results to supplies with this danger level (`safe`, `medium`, or `superdanger`)
	  * Example response:
      ```js
      { "data": [
        { "name": "Frank’s Flaming Knives", "danger": "superdanger" },
        { "name": "Hilda’s Hackeysacks", "danger": "safe" }
      ]}
      ```

  Give a jQuery code example of how a front-end developer would request the top 5 juggling supply products. **Bonus:** Use the response data to render a list of product names to the body of the page.

  ```js
  $.ajax({
    method: "GET",
    url: 'http://jugglersupply.co/api/supplies/top?limit=5',
    success: function onSuccess(response){
    	console.log(response)
    }
  })

  // bonus
  function onSuccess(response){
    var products = response.data;
    products.forEach(function(product) {
      $('body').append(product.name);
    });
  }
  ```
