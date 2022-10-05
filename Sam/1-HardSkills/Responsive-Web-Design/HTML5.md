# HTML5

Tags: HTML

### Key words | Questions

### Notes

## Headings and subheadings

---

```html
<h1>...</h1>
<h2>...</h2>
...
<h5>...</h5>
<h6>...</h6>
```

## Paragraph

---

```html
<p>...</p>
```

## Lorem Ipsum

Placeholder text

## Comments

```html
<!-- ... -->

<!-- 
... 
...
...
-->
```

## Descriptive new tags in HTML5

HTML5 introduces more descriptive HTML tags. These include `main`, `header`, `footer`, `nav`, `video`, `article`, `section` and others.

- The `main` HTML5 tag helps search engines and other developers find the main content of your page.
    
    ```html
    <main> 
      <h1>...</h1>
      <p>...</p>
    </main>
    ```
    

## Images

---

```html
<img src="httpаs://www.freecatphotoapp.com/your-image.jpg" alt="There is most be a cat photo">
<img src="httpаs://example.decorative.jpg" alt="">
```

- Note that `img` elements are self-closing
- All `img` elements **must** have an `alt` attribute. The text inside an `alt` attribute is used for screen readers to improve accessibility and is displayed if the image fails to load.
- If the image is purely decorative, using an empty `alt` attribute is a best practice.
- Ideally the `alt` attribute should not contain special characters unless needed.

## Anchor

---

```html
<a href="https://www.freecodecamp.org">this links to freecodecamp.org</a>
```

- Use `a` (*anchor*) elements to link to content outside of your web page.
- `a` elements need a destination web address called an `href` attribute. They also need anchor text.
- `a` (*anchor*) elements can also be used to create internal links to jump to different sections within a webpage.
    
    ```html
    <a href="#contacts-header">Contacts</a>
    ...
    <h2 id="contacts-header">Contacts</h2>
    ```
    
- Nest an Anchor Element within a Paragraph
    
    ```html
    <p> 
    		Here's a 
    				<a target="_blank" href="https://www.freecodecamp.org"> link to www.freecodecamp.org</a> 
    		for you.
    </p>
    ```
    
    - Attribute `target`
        - `="_blank"` open link in new window tab.
    - Attribute `href`
        - `="https://asdf.com"` URL address to the  link
- Sometimes you want to add `a` elements to your website before you know where they will link.
    
    ```html
    <p>Click here to view more <a href="#">cat photos</a>.</p>
    ```
    
- You can make elements into links by nesting them within an `a` element.
    - Remember to use `#` as your `a` element's `href` property in order to turn it into a dead link.
    
    ```html
    <a href="#">
    			<img src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/relaxing-cat.jpg" alt="Three kittens running towards the camera.">
    </a>
    ```
    

## **Lists**

---

- **Bulleted, Unordered**
    
    HTML has a special element for creating *unordered lists*, or bullet point style lists.
    
    ```html
    <ul>
    			<li>milk</li>
    			<li>cheese</li>
    </ul>
    ```
    
- **Ordered**
    
    HTML has another special element for creating *ordered lists*, or numbered lists.
    
    ```html
    <ol>
      <li>Garfield</li>
      <li>Sylvester</li>
    </ol>
    ```
    

## ****Create a Text Field****

---

`input` elements are a convenient way to get input from your user.

```html
<input type="text">
```

- Note that `input` elements are self-closing.
- Add Placeholder Text to a Text Field
    
    Placeholder text is what is displayed in your `input` element before your user has inputted anything.
    
    ```html
    <input type="text" placeholder="this is placeholder text">
    ```
    

## **Create a Form Element**

---

You can build web forms that actually submit data to a server using nothing more than pure HTML. You can do this by specifying an `action` attribute on your `form` element.

```html
<form action="url-where-you-want-to-submit-form-data">
  <input>
</form>
```

## **Add a Submit Button to a Form**

---

Clicking this button will send the data from your form to the URL you specified with your form's `action` attribute.

```html
<button type="submit">this button submits the form</button>
```

## **Use HTML5 to Require a Field**

---

For example, if you wanted to make a text input field required, you can just add the attribute `required` within your `input` element, like this: 

```html
<input type="text" required>
```

## **Create a Set of Radio Buttons**

---

You can use *radio buttons* for questions where you want the user to only give you one answer out of multiple options.

Each of your radio buttons can be nested within its own `label` element. By wrapping an `input` element inside of a `label` element it will automatically associate the radio button input with the label element surrounding it.

All related radio buttons should have the same `name` attribute to create a radio button group. By creating a radio group, selecting any single radio button will automatically deselect the other buttons within the same group ensuring only one answer is provided by the user.

- Radio buttons are a type of `input`

```html
<label> 
  <input type="radio" name="indoor-outdoor">Indoor 
</label>
```

- It is considered best practice to set a `for` attribute on the `label` element, with a value that matches the value of the `id` attribute of the `input` element. This allows assistive technologies to create a linked relationship between the label and the related `input` element. For example:
    
    ```html
    <input id="indoor" type="radio" name="indoor-outdoor">
    <label for="indoor">Indoor</label>
    ```
    

We can also nest the `input` element within the `label` tags:

```html
<label for="indoor">
	<input id="indoor" type="radio" name="indoor-outdoor">Indoor
</label>
```

## **Create a Set of Checkboxes**

---

Forms commonly use *checkboxes* for questions that may have more than one answer.

The same as radio but `type="checkbox"`

## **Use the value attribute with Radio Buttons and Checkboxes**

---

When a form gets submitted, the data is sent to the server and includes entries for the options selected. Inputs of type `radio` and `checkbox` report their values from the `value` attribute.

```html
<label for="indoor">
  <input id="indoor" value="indoor" type="radio" name="indoor-outdoor">Indoor
</label>
<label for="outdoor">
  <input id="outdoor" value="outdoor" type="radio" name="indoor-outdoor">Outdoor
</label>
```

- Here, you have two `radio` inputs. When the user submits the form with the `indoor` option selected, the form data will include the line: `indoor-outdoor=indoor`. This is from the `name` and `value` attributes of the "indoor" input.
- If you omit the `value` attribute, the submitted form data uses the default value, which is `on`. In this scenario, if the user clicked the "indoor" option and submitted the form, the resulting form data would be `indoor-outdoor=on`, which is not useful. So the `value` attribute needs to be set to something to identify the option

## **Check Radio Buttons and Checkboxes by Default**

---

You can set a checkbox or radio button to be checked by default using the `checked` attribute.

```html
<input type="radio" name="test-name" checked>
```

## **Nest Many Elements within a Single div Element**

---

The `div` element, also known as a division element, is a general purpose container for other elements.

## Meta data

---

```html
<!DOCTYPE html>
<html>
  <head>
    <meta />
  </head>
  <body>
    <div>
    </div>
  </body>
</html>
```

## **Change the Color of Text**

---

```html
<h2 style="color: blue;">CatPhotoApp</h2>
```

- Note that it is a good practice to end inline `style` declarations with a `;`.

<aside>
📌 **SUMMARY:**

</aside>