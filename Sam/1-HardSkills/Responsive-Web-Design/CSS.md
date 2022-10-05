# CSS

Tags: CSS

### Key words | Questions

### Notes

## **Change the Color of Text**

---

```html
<h2 style="color: blue;">CatPhotoApp</h2>
```

Note that it is a good practice to end inline `style` declarations with a `;`.

## ****Use CSS Selectors to Style Elements****

---

```html
<style>
  h2 {
    color: red;
  }
</style>
<!-- ... other tags -->
```

## **Use a CSS Class to Style an Element**

---

```html
<style>
  .blue-text {
    color: blue;
  }
</style>
<!-- ... other tags -->
```

Classes allow you to use the same CSS styles on multiple HTML elements

## **Change the Font Size of an Element**

---

```css
h1 {
  font-size: 30px;
}
```

## **Set the Font Family of an Element**

---

```css
h2 {
  font-family: sans-serif;
}
```

## **Import a Google Font**

---

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Lobster&display=swap" rel="stylesheet">
<!-- ... other tags -->

<style>
  h2 {
    font-family: 'Lobster', cursive;
  }
</style>
```

## **Specify How Fonts Should Degrade**

---

```css
p {
  font-family: Helvetica, sans-serif;
}
```

## **Size Your Images**

---

```css
<style>
  .larger-image {
    width: 500px;
  }
</style>
```

## **Add Borders Around Your Elements**

---

CSS borders have properties like `style`, `color` and `width`.

For example, if we wanted to create a red, 5 pixel border around an HTML element, we could use this class:

```css
<style>
  .thin-red-border {
    border-color: red;
    border-width: 5px;
    border-style: solid;
  }
</style>
```

Remember that you can apply multiple classes to an element using its `class` attribute, by separating each class name with a space. For example:

```css
<img class="class1 class2">
```

## **Add Rounded Corners with border-radius**

---

```css
.thick-green-border {
			border-radius: 10px;
			border-radius: 50%;
}
```

## **Give a Background Color to a div Element**

---

```css
.green-background {
  background-color: green;
}
```

## **Set the id of an Element**

---

There are several benefits to using `id` attributes: You can use an `id` to style a single element and later you'll learn that you can use them to select and modify specific elements with ***JavaScript***.

`id` attributes should be unique.

```html
<h2 id="cat-photo-app">
```

## ****Use an id Attribute to Style an Element****

---

```css
#cat-photo-element {
  background-color: green;
}
```

## **Adjust the Padding of an Element**

---

You may have already noticed this, but all HTML elements are essentially little rectangles. 

Three important properties control the space that surrounds each HTML element: `padding`, `border`, and `margin`. 

An element's `padding` controls the amount of space between the element's content and its `border`.

![](../Финансы/Untitled.md)

Here, we can see that the blue box and the red box are nested within the yellow box. Note that the red box has more `padding` than the blue box.

When you increase the blue box's `padding`, it will increase the distance (`padding`) between the text and the border around it.

## **Adjust the Margin of an Element**

---

An element's `margin` controls the amount of space between an element's `border` and surrounding elements.

![Here, we can see that the blue box and the red box are nested within the yellow box. Note that the red box has a bigger `margin` than the blue box, making it appear smaller.](CSS 6429e/Untitled 1.png)

Here, we can see that the blue box and the red box are nested within the yellow box. Note that the red box has a bigger `margin` than the blue box, making it appear smaller.

When you increase the blue box's `margin`, it will increase the distance between its border and surrounding elements.

## **Add a Negative Margin to an Element**

---

If you set an element's `margin` to a negative value, the element will grow larger.

## **Add Different Padding to Each Side of an Element**

---

CSS allows you to control the `padding` of all four individual sides of an element with the `padding-top`, `padding-right`, `padding-bottom`, and `padding-left` properties.

```css
.box {
			padding-top: 40px;
			padding-left: 20px;
			padding-bottom: 20px;
			padding-left: 40px;
  }
```

## **Add Different Margins to Each Side of an Element**

---

CSS allows you to control the `margin` of all four individual sides of an element with the `margin-top`, `margin-right`, `margin-bottom`, and `margin-left` properties.

```css
.box {
			margin-top: 40px;
			margin-right: 20px;
			margin-bottom: 20px;
			margin-left: 40px;
  }
```

## **Use Clockwise Notation to Specify the Padding of an Element**

---

Instead of specifying an element's `padding-top`, `padding-right`, `padding-bottom`, and `padding-left` properties individually, you can specify them all in one line, like this:

```css
padding: 10px 20px 10px 20px;
```

## **Use Clockwise Notation to Specify the Margin of an Element**

---

Instead of specifying an element's `margin-top`, `margin-right`, `margin-bottom`, and `margin-left` properties individually, you can specify them all in one line, like this:

```css
margin: 10px 20px 10px 20px;
```

## **Use Attribute Selectors to Style Elements**

---

You have been adding `id` or `class` attributes to elements that you wish to specifically style. These are known as ID and class selectors. There are other CSS Selectors you can use to select custom groups of elements to style.

```css
[type='checkbox'] {
	  margin: 10px 0px 15px 0px;
}
```

## **Understand Absolute versus Relative Units**

---

The last several challenges all set an element's margin or padding with pixels (`px`). Pixels are a type of length unit, which is what tells the browser how to size or space an item. In addition to `px`, CSS has a number of different length unit options that you can use.

The two main types of length units are absolute and relative. Absolute units tie to physical units of length. For example, `in` and `mm` refer to inches and millimeters, respectively. Absolute length units approximate the actual measurement on a screen, but there are some differences depending on a screen's resolution.

Relative units, such as `em` or `rem`, are relative to another length value. For example, `em` is based on the size of an element's font. If you use it to set the `font-size` property itself, it's relative to the parent's `font-size`.

**Note:** There are several relative unit options that are tied to the size of the viewport. They are covered in the Responsive Web Design Principles section.

## **Style the HTML Body Element**

---

We can prove that the `body` element exists here by giving it a `background-color` of black.

We can do this by adding the following to our `style` element:

```css
body {
  background-color: black;
}
```

## **Inherit Styles from the Body Element**

---

Now we've proven that every HTML page has a `body` element, and that its `body` element can also be styled with CSS.

Remember, you can style your `body` element just like any other HTML element, and all your other elements will inherit your `body` element's styles.

```css
<style>
  body {
    background-color: black;
    color: green;
    font-family: monospace;
  }
</style>

<h1> Hello World </h1>
```

## **Prioritize One Style Over Another**

---

Sometimes your HTML elements will receive multiple styles that conflict with one another.

For example, your `h1` element can't be both green and pink at the same time.

Let's see what happens when we create a class that makes text pink, then apply it to an element. Will our class *override* the `body` element's `color: green;` CSS property?

```css
<style>
  body {
    background-color: black;
    font-family: monospace;
    color: green;
  }
  .pink-text {
    color: pink;
  }
</style>

<h1 class="pink-text">Hello World!</h1>
```

## **Override Styles in Subsequent CSS**

Our `pink-text` class overrode our `body` element's CSS declaration!

We just proved that our classes will override the `body` element's CSS. So the next logical question is, what can we do to override our `pink-text` class?

```css
<style>
  body {
    background-color: black;
    font-family: monospace;
    color: green;
  }

  .pink-text {
    color: pink;
  }
  .blue-text {
    color: blue;
  }
</style>

<h1 class="pink-text blue-text">Hello World!</h1>
```

Applying multiple class attributes to a HTML element is done with a space between them like this:

```html
class="class1 class2"
```

**Note:** It doesn't matter which order the classes are listed in the HTML element.

However, the order of the `class` declarations in the `<style>` section is what is important. The second declaration will always take precedence over the first. Because `.blue-text` is declared second, it overrides the attributes of `.pink-text`

## **Override Class Declarations by Styling ID Attributes**

---

We just proved that browsers read CSS from top to bottom in order of their declaration. That means that, in the event of a conflict, the browser will use whichever CSS declaration came last. Notice that if we even had put `blue-text` before `pink-text` in our `h1` element's classes, it would still look at the declaration order and not the order of their use!

But we're not done yet. There are other ways that you can override CSS. Do you remember id attributes?

Let's override your `pink-text` and `blue-text` classes, and make your `h1` element orange, by giving the `h1` element an id and then styling that id.

```css
<style>
  body {
    background-color: black;
    font-family: monospace;
    color: green;
  }
  .pink-text {
    color: pink;
  }
  .blue-text {
    color: blue;
  }
  #orange-text {
    color: orange;
  }
</style>

<h1 class="pink-text blue-text" id="orange-text">Hello World!</h1>
```

## **Override Class Declarations with Inline Styles**

---

So we've proven that id declarations override class declarations, regardless of where they are declared in your `style` element CSS.

There are other ways that you can override CSS. Do you remember inline styles?

```css
<style>
  body {
    background-color: black;
    font-family: monospace;
    color: green;
  }
  #orange-text {
    color: orange;
  }
  .pink-text {
    color: pink;
  }
  .blue-text {
    color: blue;
  }
</style>

<h1 style="color: green;" id="orange-text" class="pink-text blue-text">Hello World!</h1>
```

## **Override All Other Styles by using Important**

---

Yay! We just proved that inline styles will override all the CSS declarations in your `style` element.

But wait. There's one last way to override CSS. This is the most powerful method of all. But before we do it, let's talk about why you would ever want to override CSS.

In many situations, you will use CSS libraries. These may accidentally override your own CSS. So when you absolutely need to be sure that an element has specific CSS, you can use `!important`.

Let's go all the way back to our `pink-text` class declaration. Remember that our `pink-text` class was overridden by subsequent class declarations, id declarations, and inline styles.

```css
color: red !important;
```

## **Use Hex Code for Specific Colors**

---

In CSS, we can use 6 hexadecimal digits to represent colors, two each for the red (R), green (G), and blue (B) components. For example, `#000000` is black and is also the lowest possible value. You can find more information about the [RGB color system here](https://www.freecodecamp.org/news/rgb-color-html-and-css-guide/#whatisthergbcolormodel)

```css
body {
  color: #000000;
}
```

## **Use Abbreviated Hex Code**

---

For example, red's hex code `#FF0000` can be shortened to `#F00`. This shortened form gives one digit for red, one digit for green, and one digit for blue.

```css
body {
  color: #000;
}
```

## **Use RGB values to Color Elements**

---

Instead of using six hexadecimal digits like you do with hex code, with `RGB`
 you specify the brightness of each color with a number between 0 and 255.

```css
body {
  background-color: rgb(255, 165, 0);
}
```

## ****Use CSS Variables to change several elements at once****

---

```css
.penguin {

    /* Only change code below this line */
    --penguin-skin: gray;
    --penguin-belly: white;
    --penguin-beak: orange;
    /* Only change code above this line */

		...
		...
		...

		.penguin-top {
	    ...
	    background: var(--penguin-skin, gray);
			...
	  }

  }
```

## **Improve Compatibility with Browser Fallbacks**

---

When working with CSS you will likely run into browser compatibility issues at some point. This is why it's important to provide browser fallbacks to avoid potential problems.

When your browser parses the CSS of a webpage, it ignores any properties that it doesn't recognize or support. For example, if you use a CSS variable to assign a background color on a site, Internet Explorer will ignore the background color because it does not support CSS variables. In that case, the browser will use whatever value it has for that property. If it can't find any other value set for that property, it will revert to the default value, which is typically not ideal.

This means that if you do want to provide a browser fallback, it's as easy as providing another more widely supported value immediately before your declaration. That way an older browser will have something to fall back on, while a newer browser will just interpret whatever declaration comes later in the cascade.

## **Inherit CSS Variables**

---

When you create a variable, it is available for you to use inside the selector in which you create it. It also is available in any of that selector's descendants. This happens because CSS variables are inherited, just like ordinary properties.

To make use of inheritance, CSS variables are often defined in the *:root* element.

`:root` is a *pseudo-class* selector that matches the root element of the document, usually the `html` element. By creating your variables in `:root`, they will be available globally and can be accessed from any other selector in the style sheet.

```css
:root {
    --penguin-belly: pink;
}
```

## **Change a variable for a specific area**

---

When you create your variables in `:root` they will set the value of that variable for the whole page.

You can then over-write these variables by setting them again within a specific element.

## **Use a media query to change a variable**

---

CSS Variables can simplify the way you use media queries.

For instance, when your screen is smaller or larger than your media query break point, you can change the value of a variable, and it will apply its style wherever it is used.

<aside>
📌 **SUMMARY:**

</aside>