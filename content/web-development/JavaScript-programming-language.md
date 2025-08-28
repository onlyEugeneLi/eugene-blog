# JavaScript Programming Language Learning Note

## `fetch(url, options)`

```js
fetch('https://YOUR_API_GATEWAY_URL', { 
    method: 'POST', 
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data) 
  })
  .then(res => res.json())
  .then(resData => {
    alert('Review submitted successfully!');
  });
```

**Flow Overview:**

1. User submits form → JS collects data.

1. fetch() sends POST request with JSON to API Gateway.

1. Browser waits for server response.

1. If successful: .then() parses JSON → show success alert.

1. If error occurs: .catch() shows error alert.

<br>

1. `fetch(url, options)`

    `fetch` is a modern browser API to send HTTP requests. The **first argument** is the URL you’re sending the request to (`YOUR_API_GATEWAY_URL`).The **second argument** is an object specifying options for the request.

1. `method: 'POST'`

    We are sending data to the server, not just asking for it.

    Common methods: GET (read), POST (create), PUT (update), DELETE.

1. `headers: { 'Content-Type': 'application/json' }`

    Tells the server that the body of the request is JSON, not plain text or form data.

    Without this, the server might not understand the data.

1. `body: JSON.stringify(data)`

    data is a JavaScript object (like `{title: "Inception", score: 9}`).

    JSON.stringify(data) converts it to a JSON string (text) that can be sent over HTTP.

1. `.then(res => res.json())`

    fetch returns a Promise (a placeholder for a value that will arrive later).

    res is the HTTP response object from the server.

    res.json() reads the response body and converts it from JSON text back into a JavaScript object.

    This also returns a Promise — that’s why we chain another .then().

1. Second `.then()`

    resData is now the parsed JSON from the server.

    Here, we just show a popup alert saying it worked.

    In a real app, you could also update the page, show the submitted review, etc.

1. `.catch()`

    Handles any errors that happen in the request, e.g., network problems or the server returning an error.

    err contains details about what went wrong.

    Here we show an alert and log it to the console for debugging.

**Quick analogy:**

`res` = the envelope with status, headers, and the letter inside.

`res.json()` = open the envelope and read the letter.

`resData` = the letter itself (the JSON data).

=> separates the parameters on the left from the function body on the right.

```js
.then(res => res.json())
```

```js
.then(function(res) {
  return res.json();
});
```

## API Calling Approaches

### Method 1: JavaScript `addEventListener("submit", …)` to handle submission

```js
document.getElementById('reviewForm').addEventListener('submit', function(e) {
  e.preventDefault();
  // collect data
  fetch('https://YOUR_API_GATEWAY_URL', { method: 'POST', body: JSON.stringify(data) })
});
```

### Method 2: Using JavaScript (onclick)

```html
<button type="button" onclick="callAPI(...)">Submit</button>
```

How it works:

The button doesn’t submit the form automatically.

Clicking the button calls a JavaScript function (callAPI) that you define.

In that function, you can use fetch() or XMLHttpRequest to send data to any API (like AWS API Gateway).

Pros:

Works well for modern AJAX-style forms — page doesn’t reload.

You can easily process the response and update the page dynamically (e.g., show result without refreshing).

Gives full control of data formatting and validation in JS.

Cons:

You must write all the JS yourself (validation, error handling, sending request).

Beginners might make small mistakes in wiring up the function.

### Method 3: Using a traditional HTML form

```html
<form action="/my-handling-form-page" method="post">
  <!-- input fields -->
  <button type="submit">Submit</button>
</form>
```

How it works:

The browser automatically sends the form data to the URL in the action attribute.

You don’t need JavaScript to send it (though you can enhance it with JS later).

Pros:

Very simple to set up — you don’t need to know JS to send data.

Works even if JavaScript is disabled in the browser.

Browser handles encoding input values and sending them.

Cons:

Page will reload by default when submitting.

Harder to show results without refreshing.

Less control over exactly how the request is formatted (JSON vs. form-encoded).