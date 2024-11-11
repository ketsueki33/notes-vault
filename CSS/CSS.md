***Map of Content***
- **[[Selectors & Specificity]]**
- **[[Position]]**
- **[[Grid Layout]]**
- **[[Flexbox Layout]]**
- **[[Responsive Design]]**
	- **[[Responsive Design#Media Queries|Media Queries]]**
	- **[[Responsive Design#Container Queries|Container Queries]]**




**Cascading Style Sheets** 
CSS (Cascading Style Sheets) is the language used to control the appearance and layout of HTML documents.

It allows developers to style web pages by setting colors, fonts, spacing, positioning, animations, and more.

CSS is essential for creating visually appealing and responsive web designs.

*Basic pattern of CSS:*
```css
selector{ 
	property : value;
};
```


**Including CSS files in HTML code**: 
using `<link>` element inside the `<head>` section. the `href` attribute specifies the location of the CSS file:
```html
<link rel="stylesheet" href="styles.css">
```

TODO: 
- **Selectors**: Used to target HTML elements to apply styles. Common selectors include class selectors (`.className`), ID selectors (`#id`), and element selectors (`elementName`).
- **Properties and Values**: Each CSS rule consists of a property (like `color` or `font-size`) and a value (like `red` or `16px`), defining how the targeted element should be styled.
- **Cascade and Specificity**: CSS follows rules for how styles are applied based on specificity (ID > class > element) and the order of appearance in stylesheets. This is known as the "cascade."
- **Box Model**: CSS treats every element as a box, with properties for margins, borders, padding, and content, which affect the element’s layout on the page.
- **Responsive Design**: Using techniques like media queries, percentages, and flexible units, CSS enables web pages to adapt to different screen sizes and devices.
- **CSS Layouts**: CSS provides layout models, such as Flexbox and Grid, for creating complex, responsive layouts without using floats or positioning tricks.