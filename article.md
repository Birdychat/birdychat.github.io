# My Journey Creating a [Telegram Chat Widget](https://birdychat.com)

After creating a chat widget JavaScript file, the embed code looked like this:

```html
<script src="https://birdychat.com/js/widget.js"></script>
<script>
  document.addEventListener('DOMContentLoaded', function () {
    init('da0a042d-78b7-4b0b-98b6-bc05a8c119c4');
  });
</script>
```

The embed code was cumbersome because it consisted of two separate parts:

* External resource loader:
```html
<script src="https://birdychat.com/js/widget.js"></script>
```

* Initialization snippet:
```html
<script>
  document.addEventListener('DOMContentLoaded', function () {
    init('da0a042d-78b7-4b0b-98b6-bc05a8c119c4');
  });
</script>
```

While this worked, I noticed that more professional embed codes consisted of a single resource loader with a data attribute:
```html
<script src="https://birdychat.com/js/widget.js" data-id="da0a042d-78b7-4b0b-98b6-bc05a8c119c4"></script>
```


## Simplified Widget Implementation

My simplified `widget.js` file looked like this:

```js
(function () {
  window.init = function (x) {
    const container = document.createElement('div');
    container.textContent = x;
    document.body.appendChild(container);
  }
})();
```

The `init` function takes a parameter `x`, creates a div element, sets its text content to `x`, and appends it to the document body. In this example, it outputs the ID string to the page. In a real-world scenario, this parameter would be used to query a database and load widget settings (such as color, position, etc.) onto the website.

## The Solution: Auto-Initialization

To simplify the embed code, I needed to add logic to read the `data-id` attribute from the resource loader:

```js
function autoInit() {
  const scriptTag = document.querySelector('script[src\*="widget.js"][data-id]');
  if (scriptTag) {
    window.init(scriptTag.dataset.id);
  }
}
document.addEventListener('DOMContentLoaded', autoInit);
```

The `autoInit` function locates the resource script tag and extracts the `data-id` from its dataset. Once extracted, the initialization runs after the `DOMContentLoaded` event fires. This approach enables the simplified embed code:

```html
<script src="https://birdychat.com/js/widget.js" data-id="da0a042d-78b7-4b0b-98b6-bc05a8c119c4"></script>
```

## Conclusion

By consolidating the embed code into a single script tag with a data attribute, I achieved a cleaner implementation that significantly improves the user experience. Users no longer need to paste multiple script blocks—they simply add one line of code to their website. This approach not only reduces complexity and potential errors but also makes the widget feel more polished and professional, similar to widely-used third-party solutions.
