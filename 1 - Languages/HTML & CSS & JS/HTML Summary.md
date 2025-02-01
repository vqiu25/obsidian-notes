# Overview of HTML Document

```html

```

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

### <span​> Tag (Span)

* **~={purple}Purpose=~**: An inline container for text. 

### <div​> Tag (Div)

* **~={purple}Purpose=~**: Block-level container for grouping elements.

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
```
```col-md
![[Pasted image 20250120173045.png | center | 100]]
```
````

### <img​> Tag (Image)

* **~={purple}Purpose=~**: Creates an image with alt text

```html
<img src="image.jpg" alt="Description of the image">
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

## Table Elements


# Attributes

# Semantic Elements

# How HTML, CSS and JS Link 