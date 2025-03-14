# Identifiers

## ID

* You can provide an `id` to any tag. This allows us to target specific tags.

```html
<p id="p1"> Paragraph Text </p>
```

```css
#p1 {
 /* CSS */
}
```

## Class

* We can provide a `class` to multiple tags. This allows us to apply the same style to everyone with the same class/

```html
<p id="p1" class="blue"> Paragraph Text </p>
```

```css
.blue {
 /* CSS */
}
```

# Styles

## Fonts


## Borders


## Background


## Margins


## Float


## Position


## Pseudo Class


## Shadows


## Icons


## Transform


## Animations



# Inline CSS

```html
<body style="background-color: #e55d5d;">
	<p>This is a <span style="color: blue;">blue</span> word.</p>
</body>
```

![[Pasted image 20250315114019.png | center | 150]]

# External CSS

## HTML

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
	<h1> This is a heading </h1>
	
	<div id="custom">
		<h1>This is a special heading</h1>
		<p> This is a paragraph </p>
	</div>
	 
</body>

</html>

```

## CSS (styles.css)

```css
body {
	background-color: black;
}

h1 {
	color: white;
}

/* In terms of CSS Evaluation, the "custom" h1 will be white, then red */
#special-section h1 {
    color: red;
    font-size: 24px;
}
```