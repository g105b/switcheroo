# Update DOM elements from the server.

A common requirement when using a serverside programming language is to update the page asynchronously, without having to refresh the whole page. This often forces one of the following decisions to be made:

1. Move or duplicate some logic to the client side JavaScript code
2. Change the serverside architecture to allow the client to build the DOM itself

This project allows you to keep all page logic on the server by switching elements with fresh ones from the server.

## Example usage: Submitting a contact form

Consider a page with a contact form. When a POST request is made, PHP replaces the form with a thank you message, or displays any validation errors. When the user submits the form, the browser sends the data to the server, the server responds with the new page, and the whole page reloads with a jolt.

```html
...

<form method="post" data-switcheroo>
	<div class="thank-you">
		<p>Thank you for your message!</p>
	</div>

	<label>
		<span>Your name</span>
		<input name="name" />
	</label>
	<label>
		<span>Your message</span>
		<textarea name="message"></textarea>
	</label>
	<label>
		<button>Send!</button>
	</label>
</form>

...
```

Dropping a `data-switcheroo` attribute on the form tells this library to perform the form submission asynchronously, add a loading indicator to the form while waiting for a response from the server, then replace the form with its thank you message when done.

**Live example: http://switcheroo.g105b.com**

The PHP, JavaScript and HTML code for the above example are available within the `example/` directory.

## Switching other elements

Sometimes submitting a form may update another element on the page, that is not within the form. Other times, a form may be submitted but it would hinder user experience to update the _whole_ form.

To update a specific element(s) when a form submits, add the CSS selector of the element(s) you wish to update to the form's `data-switcheroo` attribute. For example:

```html
<p>
	You have clicked the button <output class="update-me">0</output> times!
</p>

<form method="post" data-switcheroo=".update-me">
	<button name="do" value="increment">Increment the counter</button>
</form>
```

## Uses `fetch` behind the scenes

The asynchronous HTTP request is made using the `fetch` API. In case the browser is old and doesn't support the fetch API, the folks at Github have provided [a great polyfill that falls back to use `XMLHttpRequest`][fetch-polyfill].

[fetch-polyfill]: https://github.com/github/fetch
