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

# Layout

## Flexbox

> [!QUOTE] What is a Flexbox?
> A CSS layout model for arranging items in a **one-dimensional** row or column, allowing flexible alignment and distribution.

![[vd9dc7wfk9471.png]]

**~={red}Example=~**:

```css
header {
	display: flex;
	justify-content: space-between;
}
```

![[Group 15.png | center | 450]]

## Grid

> [!QUOTE] What is a Grid?
> A CSS layout system for creating **two-dimensional** layouts using rows and columns, providing precise control over placement.

![[phlaefsgoeb71.png]]

**~={red}Example=~**:

```html
<body>
    <div class="container"> <!-- Main grid container -->
        <header>Header</header> <!-- Top section spanning full width -->
        <nav>Navigation</nav> <!-- Sidebar navigation on the left -->
        <main>Main</main> <!-- Main content area -->
        <footer>Footer</footer> <!-- Bottom section spanning two columns -->
    </div>
</body>
```

```css
/* Define the grid container */
.container {
    display: grid; /* Enables CSS Grid */
    
    /* Define column structure: 
       - First column: Fixed 200px (Navigation)
       - Second & Third columns: Flexible (1fr each) */
    grid-template-columns: 200px 1fr 1fr;

    /* Define row structure: 
       - First row: 70px (Header)
       - Second row: Flexible (Main content)
       - Third row: 70px (Footer) */
    grid-template-rows: 70px 1fr 70px;

    /* Define grid areas for placement */
    grid-template-areas:
        "header header header"  /* Header spans all 3 columns */
        "nav main main"         /* Nav takes first column, Main spans the rest */
        "nav footer footer";    /* Nav continues, Footer spans the last two columns */
}

/* Assign each element to its corresponding grid area - provides a name */
header {
    grid-area: header;
}

nav {
    grid-area: nav;
}

main {
    grid-area: main;
}

footer {
    grid-area: footer;
}
```

![[Pasted image 20250315143435.png | center | 350]]

# Styles

## Fonts and Text Adjustments

* **~={purple}Purpose=~**: Set font for selected text. If a font is not available, it will use the next available font. We can also set the weight and style of the font.

```css
#p1 {
	font-family: "font1", "font2";
	font-style: "italic";
	font-weight: "bold";
	font-size: "18px";
}
```

## Background

* **~={purple}Purpose=~**: Background colour

```css
body {
	background: "#000000";
}
```

## Margins

* **~={purple}Purpose=~**: Space outside of border

````col
```col-md
```css
#imageOne {
	border: 5px solid;
	margin-top: 50px;
}
```
```col-md
![[Group 14.png | center | 300]]
```
````

## Padding

* **~={purple}Purpose=~**: Space between content and border

````col
```col-md
```css
#imageOne {
	padding-top: 10px;
	padding-bottom: 10px;
	padding-right: 10px;
}
```
```col-md
![[Group 14 1.png | center | 300]]
```
````

## Pseudo Class

* **~={purple}Purpose=~**: A **pseudo-class** is used to define a **special state** of an element.

### Common Pseudo Classes

|**Pseudo-class**|**Applicable Tags**|**What It’s Called**|**Example**|
|---|---|---|---|
|:hover|button, a, div, img|**Hover State** (When a user moves their cursor over an element)|button:hover { background: blue; }|
|:focus|input, textarea, button, a|**Focus State** (When an element is focused, e.g., an input field clicked)|input:focus { border: 2px solid red; }|
|:active|button, a, div|**Active State** (When an element is clicked and held)|button:active { background: green; }|
|:visited|a|**Visited State** (When a user has previously visited a link)|a:visited { color: purple; }|
|:checked|input[type="checkbox"], input[type="radio"]|**Checked State** (When a checkbox or radio button is selected)|input:checked { background: green; }|
|:disabled|input, button, textarea|**Disabled State** (When an element is disabled)|input:disabled { background: gray; }|
|:enabled|input, button, textarea|**Enabled State** (When an element is active and usable)|input:enabled { background: white; }|
|:required|input, textarea, select|**Required Field** (When an input is marked as required)|input:required { border: 2px solid red; }|
|:valid|input, textarea, select|**Valid Input** (When an input field meets validation requirements)|input:valid { border: 2px solid green; }|
|:invalid|input, textarea, select|**Invalid Input** (When an input field fails validation)|input:invalid { border: 2px solid red; }|

## Shadows

* **~={purple}Purpose=~**: Used to add **depth and dimension** to elements.

| **Property** | **Description**                                      | **Example**                                  |
| ------------ | ---------------------------------------------------- | -------------------------------------------- |
| box-shadow   | Adds shadow around an element (like a card or iamge) | box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.3); |
| text-shadow  | Adds shadow to text                                  | text-shadow: 2px 2px 5px gray;               |

**~={red}Example Explanation=~**:

| 2px  | **Horizontal offset (X-axis)** | Moves the shadow **2px to the right** (negative moves left: -2px).            |
| ---- | ------------------------------ | ----------------------------------------------------------------------------- |
| 2px  | **Vertical offset (Y-axis)**   | Moves the shadow **2px downward** (negative moves up: -2px).                  |
| 10px | **Blur radius**                | The **higher** the value, the **softer** the shadow edges (0 = sharp shadow). 

## Icons

* **~={purple}FontAwesome=~**: Library that gives us access to icons

```html
<head>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
</head>

<body>
	<i class="fa fa-home"></i> 
</body>
```

## Transform

* **~={purple}Purpose=~**: Modifies an element’s **size, position, or rotation** without affecting layout.

|**Property**|**Description**|**Example**|
|---|---|---|
|scale(n)|Enlarges or shrinks an element|transform: scale(1.2);|
|rotate(deg)|Rotates an element|transform: rotate(45deg);|
|translateX(n)|Moves element horizontally|transform: translateX(20px);|
|translateY(n)|Moves element vertically|transform: translateY(-10px);|

```css
#imageOne {
  transform: rotate(15deg);
}
```

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
#custom h1 {
    color: red;
    font-size: 24px;
}
```