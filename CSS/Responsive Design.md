Responsive design is a web design approach that ensures content looks and functions well across a range of devices, screen sizes, and orientations. It adapts layout, typography, and media based on the device's characteristics. Modern responsive design also considers user preferences (e.g., dark mode, reduced motion) to create a more accessible and personalized experience.

#### Viewport Meta Tag
This is the first responsive design concept and it has to do with HTML.

*Always use the viewport meta tag in your HTML:*
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

This `width=device-width` part ensures the layout is rendered correctly on mobile devices by setting the viewport width to the device’s width.

The `initial-scale=1.0` part sets the initial zoom level when the page is first loaded by the browser.

#### Media Queries
**Media queries** basically tells the browser to apply different CSS styles depending on a device's media type (such as print vs. screen) or other features or characteristics such as screen resolution or orientation, aspect ratio, browser viewport width or height, user preferences such as preferring reduced motion, data usage, or transparency.

It is very common to use **media queries to update custom CSS properties**(like font sizes etc.   ):
```css
:root {
    --bg-color: red;
}

/* When the viewport is alteast 600px and above */
@media (min-width: 600px) {
    :root {
        --bg-color: purple;
    }
}

body {
    background-color: var(--bg-color);
}
```

*Media Queries do not directly impact specificity* of CSS selectors within them. That is why the order of media queries that target the same element matters.

Below are some of the commonly used media queries:

##### @media for viewport width
There are many ways we can apply media queries for viewport width:

**min-width**

```css
@media (min-width: 600px) {}
```
This applies styles when the viewport width is at least equal to the specified value. 

**max-width**
```css
@media (max-width: 800px) {}
```
This applies styles when the viewport width is at most equal to the specified value. 

**width**
```css
@media (width <= 600px) {}

@media (width >= 800px) {}
```
This applies styles more intuitively based on the specified value and condition. This was introduced recently.
###### creating width ranges
We can use `min-width` and `max-width` to define styles for multiple width ranges.
```css
/* default style */
body {
    background-color: red;
}

/* between 600px and 800px */
@media (min-width: 600px) and (max-width: 800px) {
    body {
        background-color: aqua;
    }
}

/* above 800px */
@media (min-width: 800px) {
    body {
        background-color: blue;
    }
}
```

This method of creating ranges may create conflicts at interval edges. (eg. above at 800px). We can use decimal values ( eg. 799.999px ) to prevent this but a better approach is to use the latest **range syntax**:
```css
/* default style */
body {
    background-color: red;
}

/* between 600px and 800px */
@media (600px <= width < 800px) {
    body {
        background-color: aqua;
    }
}

/* above or on 800px */
@media ( 800px <= width ) {
    body {
        background-color: blue;
    }
}
```

##### @media for dark mode preference
```css
@media (prefers-color-scheme: dark){}
```
This is used to adjust our color schemes if the user has dark mode enabled on their system. 

We use it when our website doesn't have a theme switcher and have to rely on the system mode.

If our default styles are for dark mode then we can use this for adjusting to light mode:
```css
@media (prefers-color-scheme: light){}
```

##### @media for motion preference
```css
@media (prefers-reduced-motion: reduce) {}

@media (prefers-reduced-motion: no-preference) {}
```
This rule adapts animations and transitions for users who prefer minimal motion.

We can either use this rule to remove animations ( animations enabled by default ):
```css
.fancy-box {
  width: 100px;
  height: 100px;
  transform: scale(1);
  transition: transform 300ms;
}

.fancy-box:hover {
  transform: scale(1.2);
}

@media (prefers-reduced-motion: reduce) {
  .fancy-box {
    transition: none;
  }
}
```

or we can use it to add animations ( animations disabled by default ):
```css
.fancy-box {
  width: 100px;
  height: 100px;
  transform: scale(1);
  /* No more `transition` here! */
}

.fancy-box:hover {
  transform: scale(1.2);
}

@media (prefers-reduced-motion: no-preference) {
  .fancy-box {
    transition: transform 300ms;
  }
}
```

##### @media for orientation
```css
@media (orientation: landscape) {}

@media (orientation: portrait) {}
```

This is used to adjust styles based on device orientation. It is considered landscape when width is greater than height.. and portrait otherwise.

##### @media for aspect ratio
```css
/* Minimum aspect ratio */
@media (min-aspect-ratio: 8/5) {}

/* Maximum aspect ratio */
@media (max-aspect-ratio: 3/2) {}

/* Exact aspect ratio, put it at the bottom to avoid override*/
@media (aspect-ratio: 1/1) {}
```

This is used to apply styles based on the aspect ratio of the device.

##### Combining media queries
You can combine multiple media queries to create more specific conditions.

1. **Using `and`**
Combines multiple conditions, requiring all of them to be true for the styles to apply.
```css
@media (min-width: 600px) and (max-width: 800px) {}
```

2. **Using `,` (Comma)**
Creates an *OR* condition, where styles apply if any of the conditions are true. This is especially useful for targeting different devices or screen widths with the same style.

3. **Using `not`**
Negates a condition, applying styles only when the condition is **not** met.
```css
@media not (min-width: 600px) {}
```

You can use a combination of `and`, `,`, and `not` for more complex queries.

##### Nesting Media Queries
With changes brought in 2023, you can nest media queries within other styles, making it easier to organize styles for responsive design:
```css
.container {
    background: lightblue;
    
    @media (min-width: 600px) {
        background: lightcoral;
    }
}
```

#### Container Queries
Container queries are very similar to media queries for width but instead of looking at the entire viewport, this rule will look at the size of the container the element is in.

##### Defining a container
Before using container queries, you have to **define the container** so that the browser knows you might want to query the dimensions of this container later. If you have not defined any container, container queries do not fallback to the viewport.

To do this, use the `container-type` property with a value of `size`, `inline-size`, or `normal`.

**1. inline-size**
The query will be based on the inline dimensions of the container. i.e browser will be looking at the width of the container. *This is what we will be using most of the times.*
```css
.cards {
  container-type: inline-size;
}
```

**2. size**
The query will be based on the inline and block dimensions of the container. i.e browser will be looking at both width and height of the container.

**3. normal**
`container-type: normal` is useful when you want to create a container context without setting any size-related restrictions. It sets up the element so that you can still apply styles to child elements based on other attributes or properties, but without querying specific dimensions like width or height.

##### Naming containers
You can name the containers when defining them. You can then target the specified container in your container query.
```css hl=3,6
.cards {
    container-type: inline-size;
    container-name: cards;
}

@container cards (width > 45ch ) {
    .card {
        grid-template-columns: 1fr 1fr;
    }
}
```

This improves the readability and maintainability as we know which container is in responsible. It also has the following benefits:

###### Styles based on container
If the same class of elements are inside multiple different containers, we can use it to target the set of elements within a specific container:
```css
.cards {
    container-type: inline-size;
    container-name: cards;
}

.sidebar {
    container-type: inline-size;
    container-name: sidebar;
}

.card {
    background-color: black;
}

@container sidebar (width) {
    .card {
        background-color: royalblue;
    }
}
```
Here, only the card elements inside the sidebar will be blue.

###### Targeting a container within nested containers
If a element is inside multiple nested containers, then we can use named containers to specify which container should the styles depend upon.
```css hl=17
.outer-container {
    container-type: inline-size;
    container-name: outerContainer;
}

.middle-container {
    container-type: inline-size;
    container-name: middleContainer;
}

.inner-container {
    container-type: inline-size;
    container-name: innerContainer;
}

/* Targeting only the middle container */
@container middleContainer (width <= 500px) {
    .text {
        font-size: 1.5rem;
        color: blue;
    }
}
```
Here our element is inside all three containers, but we are only targeting the middle container.

###### Shorthand for defining and naming at once
```css
.cards {
    container: cards / inline-size;
}
```
This is a shorthand for the below:
```css
.cards {
    container-name: cards;
    container-type: inline-size;
}
```

##### Using container queries
Below is an example of how we can use container queries to change the layout of cards depending on the size of the container its it.
```css
.cards {
    container-type: inline-size;
}

.card {
    display: grid;
    grid-template-columns: 1fr;
}

@container (width > 45ch ) {
    .card {
        grid-template-columns: 1fr 1fr;
    }
}
```

Container queries can also be *nested* within other style rules like media queries:
```css
.cards {
    container-type: inline-size;
}

.card {
    display: grid;
    grid-template-columns: 1fr;

    @container (width > 45ch ) {
        grid-template-columns: 1fr 1fr;
    }
}
```

##### Container Query Length units
 When applying styles to a container using container queries, you can use container query length units. These units specify a length relative to the dimensions of a query container.

These are the available length units: 
- `cqw`: 1% of a query container's width
- `cqh`: 1% of a query container's height
- `cqi`: 1% of a query container's inline size
- `cqb`: 1% of a query container's block size
- `cqmin`: The smaller value of either `cqi` or `cqb`
- `cqmax`: The larger value of either `cqi` or `cqb`

These units will fallback to the viewport if browser doesn't find a defined container for it.

