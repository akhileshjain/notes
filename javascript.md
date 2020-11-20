# Event Bubbling in JavaScript

If you wrap an anchor tag inside a div and make the div clickable, then when you click on the anchor tag, action of the div will be called.
This is because event bubbles from bottom (child) element to top (parent) element and hence, when you click on the anchor tag, 
the href you defined does not open and the click action of the parent div gets triggered.

In Order to avoid this behavior, there are couple of options.
1. In the onclick event for the anchor tag, you can provide a handler and in the handler you can called the stopPropagation() method of event.
    <a href="www.google.com" onclick="handleClick(event)"></a>
    
    function handleClick(e) {
      e.stopPropagation();
    }
2. Other option is that within the tag only, you simply pass the string as below. This approach is particularly useful in React, in case the event object is not available -
    <a href="www.google.com" onclick="return false;"></a>
