# Overview of HTML Document

```html
<!DOCTYPE html>
<html lang="en">

<head> 
	<title>Page Title</title> 
	<meta charset="UTF-8"> 
	<meta name="viewport" content="width=device-width, initial-scale=1.0"> 
	<link rel="stylesheet" href="styles.css"> 
	<script src="script.js"></script> 
</head>

<body>

</body>

</html>
```

## HTML Attributes

![[Pasted image 20250314230242.png | center | 500]]

**~={purple}Attributes that set a value always have=~**:
1. A space between it and the element name (or the previous attribute, if the element already has one or more attributes).
2. The attribute name followed by an equal sign.
3. The attribute value wrapped by opening and closing quotation marks.

# HTML Elements

## Structural Elements

### Doc Type

* **~={purple}Purpose=~**: Declares the document for HTML5 so web browsers know whether to render the page in **~={green}Standard Mode=~** or **~={red}Quirks Mode=~**

```html
<!DOCTYPE html>
```

### <html​> Tag

* **~={purple}Purpose=~**: The root element that encapsulates all other HTML elements, acting as the base of the DOM tree

```html
<html>
	...
</html>
```

### <head​> Tag

* **~={purple}Purpose=~**: Contains metadata and links to external resources
	* `<title>`: Sets the title/name of the webpage
	- `<meta>`: Provides metadata (e.g., charset, viewport).
		- `charset`: Specifies the character encoding of a webpage. `UTF-8` is a universal character encoding that supports almost all characters. **~={blue}(The default is set by the browser and may not be `UTF-8`)=~**
		- `viewport`:
			- `width=device-width`: Sets the viewport width to match the device's screen width. **~={blue}(The default is desktop-friendly viewport width which looks improperly scaled on phones)=~**
			- `initial-scale=1.0`: Sets the initial zoom level (1.0 means 100%).
	- `<link>`: Links external resources like CSS.
	- `<script>`: Embeds or links JavaScript files.

```html
<head> 
	<title>Page Title</title> 
	<meta charset="UTF-8"> 
	<meta name="viewport" content="width=device-width, initial-scale=1.0"> 
	<link rel="stylesheet" href="styles.css"> 
	<script src="script.js"></script> 
</head>
```

### <body​> Tag

* **~={purple}Purpose=~**: Contains the visible content of the webpage

```html
<body>
  ...
</body>
```

## Text Elements
### <h1​>...<h6​> Tag (Headings)

* **~={purple}Purpose=~**: Represent headings, with `<h1>` being the largest and `<h6>` the smallest.

````col
```col-md
```html
<h1>Main Heading</h1>
<h2>Subheading</h2>
```
```col-md
![[Pasted image 20250120171801.png | center | 200]]
```
````

### <p​> Tag (Paragraph)

* **~={purple}Purpose=~**: Displays block of a text

````col
```col-md
```html
<p>This is a paragraph</p>
```
```col-md
![[Pasted image 20250120171957.png | center | 200]]
```
````

### Text Formatting

````col
```col-md
```html
<p>normal text</p>
<p><b>bold</b> text</p>
<p><i>italic</i> text</p>
<p><big>big</big> text</p>
<p><small>small</small> text</p>
<p><sub>subscript</sub> text</p>
<p><sup>superscript</sup> text</p>
<p><ins>inserted</ins> text</p>
<p><del>deleted</del> text</p>
<p><mark>marked</mark> text</p>
```
```col-md
![[Pasted image 20250314225744.png | center | 200]]
```
````


### <br​> Tag (Line Break)

````col
```col-md
```html
<p>This is a paragraph</p>
<br>
<p>This is a paragraph</p>
```
```col-md
![[Pasted image 20250314224258.png | center | 200]]
```
````

### <hr​> Tag (Horizontal Rule)

````col
```col-md
```html
<p>This is a paragraph</p>
<hr>
```
```col-md
![[Pasted image 20250314224327.png | center | 200]]
```
````

### <span​> Tag (Span)

* **~={purple}Purpose=~**: An inline container for text. Used to apply styles or manipulate a **small part** of text within a line.

```html
<p>This is a <span style="color: blue;">blue</span> word.</p>
```

![[Pasted image 20250314231232.png | center | 150]]

### <div​> Tag (Div)

* **Purpose**: Block-level container for grouping elements. Used to group multiple elements for styling, layout, or scripting.

```html
<!-- Styling is applied to all HTML elements within div tag -->
<div style="background-color: lightblue; padding: 10px;">
  <h2>Section Title</h2>
  <p>This is a paragraph inside a div.</p>
</div>
```

![[Pasted image 20250314231424.png | center | 200]]

## List Elements
### <ul​> Tag (Unordered List)

````col
```col-md
```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```
```col-md
![[Pasted image 20250120172500.png | center | 100]]
```
````

### <ol​> Tag (Ordered List)

````col
```col-md
```html
<ol>
  <li>First item</li>
  <li>Second item</li>
</ol>
```
```col-md
![[Pasted image 20250120172624.png | center | 150]]
```
````

## Link & Media Elements
### <a​> Tag (Anchor)

* **~={purple}Purpose=~**: Creates hyperlinks

````col
```col-md
```html
<a href="google.com">Visit Google</a>

 <!-- Open Link in New Tab -->
<a href="google.com" target=_blank>Visit Google</a>
```
```col-md
![[Pasted image 20250120173045.png | center | 100]]
```
````

````col
```col-md
```html
<a href="google.com" target=_self title="Hello">Visit Google</a>
```
```col-md
![[Pasted image 20250314224808.png | center | 100]]
```
````

````col
```col-md
```html
 <!-- Relative HTML Page -->
<a href="page2.html">Visit Page 2</a>
```
```col-md
![[Pasted image 20250314225039.png | center | 100]]
```
````

### <img​> Tag (Image)

* **~={purple}Purpose=~**: Creates an image with alt text

```html
<img src="image.jpg" alt="Description of the image" height="200">
```

* We can make our image a hyperlink via:

```html
<a href="google.com">
<img src="image.jpg" alt="Description of the image" height="200">
</a>
```

### <video​> Tag (Video)

* **~={purple}Purpose=~**: Embeds a video

```html
<video controls>
  <source src="video.mp4" type="video/mp4">
  Your browser does not support video.
</video>
```

### <audio​> Tag (Audio)

* **~={purple}Purpose=~**: Embeds audio

```html
<audio controls>
  <source src="audio.mp3" type="audio/mpeg">
  Your browser does not support audio.
</audio>
```

## Interactive Elements

### <form​> Tag (Form)

* **~={purple}Purpose=~**: Create form for user post

```html
<form action="/submit" method="POST">
  <input type="text" name="username">
  <button type="submit">Submit</button>
</form>
```

![[Pasted image 20250314232409.png | center | 200]]

```html
<form>
	<label for="firstname">first name:</label>
	<input type="text" id="firstname" name="firstname" value="Victor"><br><br>

	<label for="lastname">last name:</label>
	<input type="text" id="lastname" name="lastname" value="Qiu"><br><br>

	<label for="password">password:</label>
	<input type="password" id="password" name="password"><br><br>

	<!-- Read only -->
	<label for="email">email:</label>
	<input type="email" id="email" name="email" value="name@gmail.com" readonly><br><br>

	<label for="phone">phone #:</label>
	<input type="tel" id="phone" name="phone" value="(123)-456-7890"><br><br>

	<label for="birthdate">birthdate:</label>
	<input type="date" id="birthdate" name="birthdate"><br><br>

	<label for="quantity">quantity:</label>
	<input type="number" id="quantity" name="quantity" value="1"><br><br>

	<label>title:</label>
	<input type="radio" id="mr" name="title" value="Mr.">
	<label for="mr">Mr.</label>
	<input type="radio" id="mrs" name="title" value="Mrs.">
	<label for="mrs">Mrs.</label>
	<input type="radio" id="phd" name="title" value="PhD.">
	<label for="phd">PhD.</label><br><br>

	<label for="payment">payment:</label>
	<select id="payment" name="payment">
		<option value="visa">visa</option>
		<option value="mastercard">mastercard</option>
		<option value="paypal">paypal</option>
	</select><br><br>

	<label for="subscribe">subscribe:</label>
	<input type="checkbox" id="subscribe" name="subscribe" checked><br><br>

	<button type="reset">Reset</button>
	<button type="submit">Submit</button>
</form>
```

![[Pasted image 20250314232852.png | center |200]]

## Table Elements

* `<table bgcolor="">`: Border Colour
* `<tr bgcolor="">`: Background Colour

````col
```col-md
```html
<table bgcolor="black">
    <tr bgcolor="grey">
        <th>Sunday</th>
        <th>Monday</th>
        <th>Tuesday</th>
        <th>Wednesday</th>
        <th>Thursday</th>
        <th>Friday</th>
        <th>Saturday</th>
    </tr>
    <tr bgcolor="lightgrey" align="center">
        <td>Closed</td>
        <td>9-5</td>
        <td>9-5</td>
        <td>9-5</td>
        <td>9-5</td>
        <td>9-5</td>
        <td>Closed</td>
    </tr>
</table>
```
```col-md
![[Pasted image 20250314230606.png]]
```
````

# In-Line CSS

The style attribute is added inside an HTML tag, with CSS properties written as `property: value;` pairs.

```html
<tag style="property: value; property2: value2">
```

