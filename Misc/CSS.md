# CSS

### What are the different types of Selectors in CSS?

CSS Selectors are powerful tools that allows us to target specific HTML elements and apply styles to them.

- Universal Selector

The universal selector selects all elements on the page. It can be used to apply global styles.

Ex: Bellow code applies margin & padding of 0 on all the elements.
![Alt text](css/image.png)

- Element Type Selector

The type selector selects all HTML elements of a specific type.

Ex: To style all paragraphs on a page, we can use the `p` selector.
![Alt text](css/image-1.png)

- Class Selector

The class selector targets elements with a specific 'class' attribute. Denoted by a dot followed by the class name.

Ex: Below code style elements with a class of box
![Alt text](css/image-2.png)

- ID Selector

The ID selector targets elements based on their unique ID attribute. Denoted by a hash symbol followed by the ID name.

Ex: To target an element with the ID "logo", we can use the `#logo` selector.

![Alt text](css/image-3.png)

- Descendant Selector

It selects elements that are descendants of another element at any depth. Denoted by a space between two selectors

Ex: Red color is applied to both paragraphs ABC & XYZ

![Alt text](css/image-4.png)

- Child Selector

It's similar to descendant selector but it only selects elements that are direct children of another element. Denoted by a > symbol between two selectors

Ex: Red color is only applied to Item 1 & Item 2
![Alt text](css/image-5.png)

- Adjacent Sibling Selector

It selects elements that are siblings & immediately follow another element. Denoted by a + sign between two selectors

Ex: `h2 + p` selects para that directly follows an h2 heading
![Alt text](css/image-6.png)

---

### Flexbox and its properties

Flexbox is a layout model in CSS that allows you to create flexible and responsive layouts.

- `display: flex;`: defines it as a flex container.

- `flex-direction`: Determines the main axis direction. It can be set to `row`, `column`, `row-reverse`, or `column-reverse`.

- `justify-content`: Defines how items are distributed along the main axis. Values include `flex-start`, `flex-end`, `center`, `space-between`, `space-around`, and `space-evenly`.

- `align-items`: Specifies how items are aligned along the cross-axis. Values include `flex-start`, `flex-end`, `center`, `baseline`, and `stretch`.

- `flex`: Allows you to control how items grow or shrink in relation to each other.

- `align-self`: Overrides the `align-items` property for individual items.

- `order`: Specifies the order in which the items appear within the container.

- `flex-wrap`: Controls whether items wrap onto multiple lines or not.

---

### Position property

The `position` property in CSS is used to control the positioning of elements. It has several values:

- `static`: This is the default value. The element follows the normal flow of the document.

- `relative`: The element is positioned relative to its normal position. You can use `top`, `right`, `bottom`, and `left` properties to adjust its position.

- `absolute`: The element is removed from the normal flow and positioned relative to the nearest positioned ancestor (or the document itself if none is found). It won't affect other elements' positions.

- `fixed`: The element is positioned relative to the viewport, meaning it will stay in the same position even if the page is scrolled.

- `sticky`: The element behaves like `relative` until it reaches a certain threshold (defined by `top`, `right`, `bottom`, or `left`), then it becomes `fixed`.

---

### How to implement media queries:

Media queries allow you to apply different CSS styles based on the characteristics of the user's device or screen size.

Here's the basic syntax for a media query:

```css
/* Standard CSS styles for all viewports */

/* Media query for viewports with a maximum width of 768 pixels */
@media screen and (max-width: 768px) {
  /* CSS styles for viewports with a maximum width of 768 pixels */
}

/* Media query for viewports with a minimum width of 768 pixels */
@media screen and (min-width: 768px) {
  /* CSS styles for viewports with a minimum width of 768 pixels */
}
```

You can adjust the `max-width` and `min-width` values to target different screen sizes and apply specific styles accordingly.

---

### Center a div:

To center a `div` horizontally and vertically within its parent container, you can use the following CSS:

```css
.container {
  /* Set the container to flex to align the div */
  display: flex;
  /* Center the div along the main (horizontal) axis */
  justify-content: center;
  /* Center the div along the cross (vertical) axis */
  align-items: center;
  /* Add a fixed height to the container to center vertically, or use height: 100% on the parent element */
  height: 300px;
}

.centered-div {
  /* Add styling for the centered div here */
}
```

---

### What is a Grid system in CSS?

CSS splits the page into grids and utilizes those grids to handle the HTML content.
Utilizing the Grids, CSS can stack and highlight various elements in different parts of
the grids.

---

### Explain user-centered design?

An iterative design procedure, User-centred design lets the designers focus on the
clients and their needs in every design process phase. The user-centered design calls
for linking users in the design process via a variability of design and research
techniques to make usable and highly accessible products. User-centered design
demands that designers should utilize a combination of generative (such as
brainstorming) and investigative (interviews and surveys) methods and instruments
to create an understanding of user requirements.

---

### What is box model ?

Content, Padding, Border, Margin

---

### What do you know about the CSS image sprites and why it is

utilized?

CSS image sprites assist to render numerous images in a single line image. In a
nutshell, the CSS sprites merge numerous photos into a single large image. If a web
page comprises different images, then it would raise its loading time as for every
image the browser has to send a distinct HTTP request, but with the help of sprites,
we have a single image to request

---

### Mention the pitfalls for using a CSS Preprocessor like Sass?

- An extra tool for the preprocessor is required.
- Preprocessor files can not be performed directly on the browser.
- Slow re-compilation of the preprocessor.
- For the preprocessor, you ought to know extra tools, which improve the learning curve of CSS.

---

### Usage of !important in CSS:

`!important` is a keyword in CSS that can be applied to a CSS property to give it the highest specificity, making it override other conflicting styles.

---

### What is pseudo element in CSS ?

A pseudo-element in CSS is a way to style a specific part of an element's content or layout without changing the HTML structure. It is denoted by double colons (::) and is different from pseudo-classes that use a single colon (:). Pseudo-elements like ::before and ::after are commonly used to add decorative elements or modify the appearance of elements dynamically, enhancing the design without cluttering the HTML code.

```css
p::before {
  content: ">> "; /* This adds ">> " before the content of all <p> elements */
}
```

---

### Different ways by which we could hide an element and which of this is the best ?


a. `display: none;`: This completely removes the element from the page flow, and it will not occupy any space.

```css
.hidden-element {
  display: none;
}
```

b. `visibility: hidden;`: This hides the element, but it still occupies space on the page. 

```css
.hidden-element {
  visibility: hidden;
}
```

c. `opacity: 0;`: This makes the element fully transparent, effectively hiding it while still occupying space and maintaining its layout.

```css
.hidden-element {
  opacity: 0;
}
```

d. `position: absolute;` (or `fixed`) with `top`, `right`, `bottom`, or `left` set to positions outside the visible area: This technique positions the element outside the viewport, effectively hiding it from view. The element still exists in the page flow.

```css
.hidden-element {
  position: absolute;
  top: -9999px;
  /* or
  left: -9999px;
  right: -9999px;
  bottom: -9999px;
  */
}
```

e. Using the `visibility` property with `height: 0` and `overflow: hidden`: This approach hides the element but maintains its layout.

```css
.hidden-element {
  visibility: hidden;
  height: 0;
  overflow: hidden;
}
```
