---
title:  "Angular - Convert any Html element to Image using canvas"
categories: 
  - Javascript
tags:
  - Angular
comments: true
---

Sometimes you need to convert your entire div or web page to an image. So, I am going to convert a div or HTML to image with using angular module [**html2canvas** ](https://html2canvas.hertzen.com/)

 The screenshot is based on the DOM and as such may not be 100% accurate to the real representation as it does not make an actual screenshot, but builds the screenshot based on the information available on the page. The library should work fine on the following browsers (with Promise polyfill):
    -   Firefox 3.5+
    -   Google Chrome
    -   Opera 12+
    -   IE9+
    -   Safari 6+

As each CSS property needs to be manually built to be supported, there are a number of properties that are not yet supported. It does not require any rendering from the server, as the whole image is created on the clients browser. It doesn't magically circumvent any browser content policy restrictions either, so rendering cross-origin content will require a proxy to get the content to the same origin.

## Requirements

Install the library in the application using **NPM** or if you prefer **Bower**.

```javascript
    npm isntall html2canvas

    // or

    bower install html2canvas
```
Incase if you are using this for a plain javascript web application then you can simply add the reference to the script with a script tag. [html2canvas.min.js](https://html2canvas.hertzen.com/)

```javascript
    <script src="/path/to/html2canvas.min.js"></script>
```

## Create Screenshot using html2canvas

Let's say you want to capture screenshot of div in you web application. Example, in the below image the black highlighted part is the div you need to capture. So first make sure you hav set an **id** to the element. 

[![screenshot1](https://prasanthj.com/assets/uploads/html2canvas-1.png)](https://prasanthj.com/assets/uploads/html2canvas-1.png)

The **html2canvas** is a simple library, the function expects an argument which is the DOM element of the web page in other words the DOM element which you want to export as image. 

```javascript
    let element = document.querySelector("#capture");
    html2canvas(document).then(function(canvas) {
        // Convert the canvas to blob
        canvas.toBlob(function(blob){
            // To download directly on browser default 'downloads' location
            let link = document.createElement("a");
            link.download = "image.png";
            link.href = URL.createObjectURL(blob);
            link.click();

            // To save manually somewhere in file explorer
            window.saveAs(blob, 'image.png');

        },'image/png');
    });
```

Once the element is passed to the **html2canvas** window function then a promise is returned with the **canvas** of the div. Use the function **canvas.toBlob()** to convert it to the canvas to a blob and from blob you can create a url out of it using **URL.createObjectURL()**. 

To download automatically to default browser 'Downloads' location, Create an anchor element and assign the link to the href and make a click so it will automatically download the data.

To allow user to save manually in their machine, use **window.saveAs()** function.

The downloaded image will be like this finally..

[![screenshot2](https://prasanthj.com/assets/uploads/html2canvas-2.png)](https://prasanthj.com/assets/uploads/html2canvas-2.png)