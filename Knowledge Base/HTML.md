***Map of Content***
- [[#Introduction]]
- [[#HTML elements]] 
- [[#Anatomy of an HTML Document]] 
- [[#Meta Tags]] 
- [[#`<link>` element]]
- [[#Non-Semantic Elements]] 
- [[#Semantic Elements]] 
- [[#Tables]]
- [[#Forms & Inputs]]
- [[#Hyperlinks - `<a>` element]]
- [[#Entities]]

----

#### Introduction

HTML, or **HyperText Markup Language**, is a _markup language_ that defines the structure of a web page. HTML consists of a series of elements, which you use to enclose, or wrap, different parts of the content to make it appear a certain way, or act a certain way.

HTML consists of a series of elements, which you use to enclose, or wrap, different parts of the content to make it appear a certain way, or act a certain way.

#### HTML elements
HTML consists of a series of elements, which you use to enclose, or wrap, different parts of the content to make it appear a certain way, or act a certain way.

Below is an example of an HTML element:
```html
<p>My cat is very grumpy</p>
```

##### Anatomy of an HTML element
![|400](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics/grumpy-cat-small.png)

The main parts of our element are as follows:

1. **The opening tag:** This consists of the name of the element (in this case, p), wrapped in opening and closing **angle brackets**. This states where the element begins or starts to take effect — in this case where the paragraph begins.
2. **The closing tag:** This is the same as the opening tag, except that it includes a _forward slash_ before the element name. This states where the element ends — in this case where the paragraph ends. Failing to add a closing tag is one of the standard beginner errors and can lead to strange results.
3. **The content:** This is the content of the element, which in this case, is just text.
4. **The element:** The opening tag, the closing tag, and the content together comprise the element.
##### Attributes
Attributes contain extra information about the element that you don't want to appear in the actual content.
```html
<p class="editor-note">My cat is very grumpy</p>
```
Here, `class` is the attribute _name_ and `editor-note` is the attribute _value_. The `class` attribute allows you to give the element a non-unique identifier that can be used to target it (and any other elements with the same `class` value) with style information and other things. Some attributes have no value, such as `required`.

##### Void elements

Some elements have no content and are called void elements. Take the `<img>` element for example:
```html
<img src="images/firefox-icon.png" alt="My test image" />
```

This contains two attributes, but there is no closing `</img>` tag and no inner content. This is because an image element doesn't wrap content to affect it. 

Its purpose is to embed an image in the HTML page in the place it appears.

#### Anatomy of an HTML Document
This is how a basic HTML document looks like: 
```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>My test page</title>
  </head>
  <body>
    <img src="images/firefox-icon.png" alt="My test image" />
  </body>
</html>
```

##### 1. doctype
All HTML documents **must start** with a `<!DOCTYPE>` declaration.

The declaration is not an HTML tag. It is an "information" to the browser about what document type to expect.

In HTML 5, the declaration is simple:
```html
<!DOCTYPE html>
```

In older documents (HTML 4 or XHTML), the declaration is more complicated because the declaration must refer to a DTD (Document Type Definition).

HTML 4.01:
```html
 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

XHTML 1.1:
```html
 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```

| Feature                  | HTML4.01         | XHTML 1.1          | HTML5                                   |
| ------------------------ | ---------------- | ------------------ | --------------------------------------- |
| **Syntax Flexibility**   | Moderate         | Strict (XML-based) | Flexible and forgiving                  |
| **Semantic Tags**        | Limited          | Limited            | Extensive                               |
| **Multimedia Support**   | Requires plugins | Limited            | Native support for audio, video         |
| **APIs for Modern Web**  | None             | None               | Geolocation, Canvas, LocalStorage, etc. |
| **Error Handling**       | More forgiving   | Strict             | More forgiving                          |
| **Mobile Compatibility** | Limited          | Limited            | Designed with mobile in mind            |

> [!warning] What if we don't declare DOCTYPE?
> If the DOCTYPE declaration is missing, the browser will open our web page in **quirks mode**.
> 
> Quirks mode is an approach used by web browsers to maintain *backward compatibility* with web pages designed for old web browsers, instead of strictly complying with web standards in standards mode. This means some elements may not work as intended in this mode.

##### 2. `<html>` element
This element wraps all the content on the entire page and is sometimes known as the root element. It also includes the `lang` attribute, setting the primary language of the document.

##### 3. `<head>` element
This element acts as a container for all the stuff you want to include on the HTML page that _isn't_ the content you are showing to your page's viewers. This includes things like keywords and a page description that you want to appear in search results, CSS to style our content (see [[#`<link>` element|link element]]), character set declarations and other [[#Meta Tags|meta tags]].

##### 4. `<title>` element
This sets the title of your page, which is the title that appears in the browser tab the page is loaded in. It is also used to describe the page when you bookmark/favorite it.

##### 5. `<body>` element
This contains _all_ the content that you want to show to web users when they visit your page, whether that's text, images, videos, games, playable audio tracks, or whatever else.

#### Meta Tags
Meta tags are HTML elements that provide information about your web pages to search engines and browsers.

They go in the `<head>` section of your page.

###### Meta Charset tag
```html
<meta charset="utf-8">
```
This element sets the character set your document should use.

Most common character set is `UTF-8` which includes most characters from the vast majority of written languages.

###### Meta Viewport tag
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
This gives the browser instructions on how to control the page's dimensions and scaling.

The `width=device-width` part sets the width of the page to follow the screen-width of the device (which will vary depending on the device).

The `initial-scale=1.0` part sets the initial zoom level when the page is first loaded by the browser.

###### Meta Keywords tag
```html
<meta name="keywords" content="HTML, CSS, JavaScript">
```

Defines keywords related to the content of the page. This tag is less influential now as search engines focus more on content than keywords.

###### Meta Description tag
```html
<meta name="description" content="Learn web development and design with our easy-to-follow tutorials.">
```
Provides a brief description of the page, which search engines use to display in search results.

###### Meta Author tag
```html
<meta name="author" content="ketsueki">
```

###### Meta Robots tag
```html
<meta name="robots" content="index, follow">
```
Gives instructions to search engine crawlers about indexing and following links on the page.
- **index** tells search engines to index the page.
- **follow** allows search engines to follow links on the page.
- Use **noindex** or **nofollow** if you want to restrict certain pages from being indexed or crawled.
###### Meta Refresh/Redirect Tag
```html
<meta http-equiv="refresh" content="30">
```
Refresh page every 30 seconds.

```html
<meta http-equiv="refresh" content="5; url=https://example.com">
```
redirects the page to another URL after 5 seconds.

###### Meta OG(Open Graph) tags
```html
<meta property="og:title" content="Web Development Tutorials">
<meta property="og:description" content="Learn web development with tutorials on HTML, CSS, and JavaScript.">
<meta property="og:image" content="https://example.com/thumbnail.jpg">
<meta property="og:url" content="https://example.com">
```
Define content that social media platforms use when the page is shared. Useful for controlling how the page appears on platforms like Facebook and LinkedIn.

###### Meta Twitter Card tags
```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Web Development Tutorials">
<meta name="twitter:description" content="Learn web development with tutorials on HTML, CSS, and JavaScript.">
<meta name="twitter:image" content="https://example.com/thumbnail.jpg">
```
Similar to Open Graph, but for Twitter-specific sharing customization.

#### `<link>` element
The `<link>` element in HTML primarily defines a relationship between an HTML document and an external resource, often used to link stylesheets and other external resources. It is most commonly defined inside the `<head>` tag.

Here’s a rundown of its common uses:

**Linking Stylesheets**
The most common use for `<link>` is to link external CSS files.
```html
<link rel="stylesheet" href="styles.css">
```

**Favicon**
 Specifies the icon shown in the browser tab.
 ```html
 <link rel="icon" href="favicon.ico" type="image/x-icon">
```

**Preloading and Prefetching Resources**
 Instructs the browser to load resources in advance to improve performance.
- *Types*:
    - `preload`: Preloads resources for current navigation (e.g., fonts, images, styles).
    - `prefetch`: Preloads resources for likely future navigations.
```html
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin="anonymous">
<link rel="prefetch" href="future-page.html">
```

#### Non-Semantic Elements
Non-semantic elements are used to create structure and presentation for web pages. They don't provide any information about the content of the web page. They are used to define sections of a web page and apply styles to the content.

##### `<div>`
It is used to group content together and apply styles to the content. 

**It is a block level element.**

For example, you could use the `<div>` element to group all the content on your web page together and apply a background color to it. 
```html
<body>
    <div style="background-color: red">
        <h1>Welcome to My Website</h1>
        <p>Here you will find information about my services.</p>
    </div>
    <ul>
        <li>Service 1</li>
        <li>Service 2</li>
        <li>Service 3</li>
    </ul>
</body>
```

##### `<span>`
It is used to apply styles to specific parts of the content, such as text or images. 

**It is an inline element.**

For example, you could use the `<span>` element to apply a different color to a specific word in a sentence.
```html
<p>This is a <span style="color: red;">red</span> word.</p>
```

##### Semantic vs Non-Semantic Elements
> [!NOTE] Difference between Semantic and Non-Semantic Elements
> The main difference between semantic and non-semantic elements is that semantic elements provide meaning to the content, while non-semantic elements do not.
> 
> Semantic elements, such as `<header>`, `<nav>`, `<section>`, and `<footer>`, are used to define the structure of a web page and give meaning to the content. 
> 
> Non-semantic elements, such as `<div>` and `<span>`, are used for layout and formatting purposes only.

#### Semantic Elements
Semantic elements are HTML tags that describe the meaning of content on a web page to both the browser and the developer. They are essential for creating accessible, well-structured, and SEO-friendly web pages.

*Using semantic elements has several benefits, including:*

- Improved accessibility for assistive technologies like screen readers.
- Better search engine optimization (SEO).
- Clearer and more concise HTML code.
- More organized and maintainable code.

| Tag            | Description                                                                                       |
|----------------|---------------------------------------------------------------------------------------------------|
| `<article>`    | Defines independent, self-contained content such as a blog post, news article.                    |
| `<aside>`      | Defines content aside from the main content of the page                                           |
| `<details>`    | Defines additional details that the user can view or hide                                         |
| `<figcaption>` | Defines a caption for a `<figure>` element                                                        |
| `<figure>`     | Specifies self-contained content, like illustrations, diagrams, photos, code listings, etc.       |
| `<footer>`     | Defines a footer for a document or section                                                        |
| `<header>`     | Specifies a header for a document or section                                                      |
| `<main>`       | Specifies the main content of a document                                                          |
| `<mark>`       | Defines marked/highlighted text                                                                   |
| `<nav>`        | Defines navigation links                                                                          |
| `<section>`    | Defines a section in a document                                                                   |
| `<summary>`    | Defines a visible heading for a `<details>` element                                               |
| `<time>`       | Defines a date/time                                                                               |
| `<address>`    | Provides contact information for the author or owner of a document                                |
| `<blockquote>` | Defines a section that is quoted from another source                                              |
| `<cite>`       | Represents the title of a work, such as a book, play, or song                                     |
| `<code>`       | Defines a fragment of computer code                                                               |
| `<em>`         | Emphasizes text (italicized by default)                                                           |
| `<strong>`     | Indicates important text (bolded by default)                                                      |
| `<kbd>`        | Represents user input (usually keyboard input)                                                    |
| `<samp>`       | Defines sample output from a program or system                                                    |
| `<output>`     | Represents the result of a calculation or user action                                             |
| `<dfn>`        | Represents the defining instance of a term                                                        |
| `<abbr>`       | Represents an abbreviation or acronym, often with a title attribute providing expanded meaning    |

#### Tables
HTML tables allow web developers to arrange data into rows and columns.

| Tag/Attribute | Description                                                                                 |
| ------------- | ------------------------------------------------------------------------------------------- |
| `<table>`     | Declares the start of a table.                                                              |
| `<td>`        | Defines a cell of a table that contains data.                                               |
| `<tr>`        | Represents a row of cells.                                                                  |
| `<th>`        | Defines a header cell in a table, typically bold and centered by default.                   |
| `<thead>`     | Defines a set of rows that represent the head of the columns, usually with `<th>` tags.     |
| `<tfoot>`     | Similar to `<thead>`, but defines a footer at the bottom of the table.                      |
| `<tbody>`     | Defines the main body of the table, containing the standard data rows.                      |
| `rowspan="x"` | Specifies the number of rows a cell will span across (an attribute of `<td>` or `<th>`).    |
| `colspan="x"` | Specifies the number of columns a cell will span across (an attribute of `<td>` or `<th>`). |

**Example:**
<table border="1">
    <thead>
        <tr>
            <th colspan="3">Employee Information</th>
        </tr>
        <tr>
            <th>Name</th>
            <th>Position</th>
            <th>Details</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="2">Alice</td>
            <td>Developer</td>
            <td>Experienced in frontend and backend development.</td>
        </tr>
        <tr>
            <td>Team Lead</td>
            <td>Leading a team of 5 developers.</td>
        </tr>
        <tr>
            <td>Bob</td>
            <td colspan="2">Project Manager, overseeing multiple projects.</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="3">End of Employee Information</td>
        </tr>
    </tfoot>
</table>

The HTML for the above table is: 
```html
<table border="1">
    <thead>
        <tr>
            <th colspan="3">Employee Information</th>
        </tr>
        <tr>
            <th>Name</th>
            <th>Position</th>
            <th>Details</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="2">Alice</td>
            <td>Developer</td>
            <td>Experienced in frontend and backend development.</td>
        </tr>
        <tr>
            <td>Team Lead</td>
            <td>Leading a team of 5 developers.</td>
        </tr>
        <tr>
            <td>Bob</td>
            <td colspan="2">Project Manager, overseeing multiple projects.</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="3">End of Employee Information</td>
        </tr>
    </tfoot>
</table>
```

#### Forms & Inputs
An HTML form is used to collect user input. The user input is most often sent to a server for processing.

##### `<form>` element
This element itself is a shell or container that doesn’t have any visual impact.  This element “represents a document section containing interactive controls for submitting information”.

 We fill the form with a collection of different inputs and buttons.
 
| Attribute        | Description                                                                                                 | Example Value                    |
|------------------|-------------------------------------------------------------------------------------------------------------|----------------------------------|
| `action`         | Specifies the URL where the form data will be sent.                                                         | `/submit-form`                   |
| `method`         | Defines the HTTP method used to send form data, usually `GET` or `POST`.                                    | `post`                           |
| `enctype`        | Specifies the encoding type of the form data when submitted. Commonly used with file uploads.               | `multipart/form-data`            |
| `target`         | Determines where to display the response, such as in a new window or frame.                                 | `_blank`                         |
| `autocomplete`   | Enables or disables the browser's autocomplete feature.                                                     | `on`                             |
| `novalidate`     | Disables HTML5 form validation when the form is submitted.                                                  | `novalidate`                     |
| `name`           | Assigns a name to the form, which can be used for scripting or styling.                                     | `contactForm`                    |                       |
| `rel`            | Indicates the relationship between the current document and the form target, useful in external submissions. | `noopener`                       |


##### `<input>` element

The `<input>` tag specifies an input field where the user can enter data.

The `<input>` element can be displayed in several ways, depending on the type attribute.

The different input types are as follows:

| Input Type          | Description                                                                               |
|---------------------|-------------------------------------------------------------------------------------------|
| `text`              | A single-line text input. (*default value*)                                               |
| `password`          | A single-line text input that hides the entered characters.                               |
| `email`             | A single-line text input for email addresses, with validation for correct format.         |
| `url`               | A single-line text input for URLs, with validation for correct format.                   |
| `number`            | A numeric input field that allows users to enter numbers.                                 |
| `range`             | A slider input for selecting a numeric value within a specified range.                   |
| `tel`               | A single-line text input for telephone numbers.                                          |
| `checkbox`          | A checkbox that allows for binary (true/false) selections.                               |
| `radio`             | A set of radio buttons that allows a user to select one option from a set.              |
| `file`              | A file upload input that allows users to select files for upload.                         |
| `color`             | A color picker input that allows users to select a color.                                |
| `date`              | A date input for selecting dates from a calendar.                                       |
| `datetime-local`    | A date and time input for selecting both date and time.                                  |
| `month`             | A month input for selecting a specific month and year.                                   |
| `week`              | A week input for selecting a specific week of the year.                                  |
| `hidden`            | A hidden input that is not visible to users but can hold data to be submitted.           |

Depending on the value of `type` attribute, you will have access to other sets of attributes.

##### `<textarea>` element
A multi-line text input for longer text entries.

The size of a text area is specified by the `cols` and `rows` attributes (or with CSS).

##### `<select>` element for drop-down
The `<select>` element is used to create a drop-down list.

The `<option>` tags inside the `<select>` element define the available options in the drop-down list.

**Example:**
```html
<label for="cars">Choose a car:</label>

<select name="cars" id="cars">
  <option value="volvo">Volvo</option>
  <option value="saab">Saab</option>
  <option value="mercedes">Mercedes</option>
  <option value="audi">Audi</option>
</select> 
```

##### `<label>` element
The `<label>` tag defines a label for input elements.

Benefits on using `<label>` for inputs:
- Screen reader users (will read out loud the label, when the user is focused on the element)
- Users who have difficulty clicking on very small regions (such as checkboxes) - because when a user clicks the text within the `<label>` element, it toggles the input (this increases the hit area).


> [!important] Binding label and input
> The for attribute of `<label>` must be equal to the id attribute of the related element to bind them together. A label can also be bound to an element by placing the element inside the `<label>` element.

##### `<button>` element for forms
The `<button>` tag defines a clickable button. When placed inside a form, it can trigger form actions.

Use the `type` attribute to specify the behaviour:
- **submit**: button submits the form data to the server.
-  **reset**:  button resets all form fields to their default values.

#### Hyperlinks - `<a>` element
The `<a>` ( called an anchor tag) tag defines a hyperlink, which is used to link from one page to another.

The most important attribute of the `<a>` element is the `href` attribute, which indicates the link's destination.

By default, links will appear as follows in all browsers:
- An unvisited link is underlined and blue
- A visited link is underlined and purple
- An active link is underlined and red


###### Jumping to a section/fragment
Anchor element can be used to jump to sections on the same webpage. This can be done by first giving an element a unique identifier with `id="uid"`. This is called a fragment. 

`<a href=”#uid”> jump </a>` : this will jump us to our fragment.

  
`<a href=”#”>  to top </a>`: an empty fragment will always jump to the top.

###### Opening in new tab
```html
<a href=”https://google.com” target=”_blank”> target=”_blank”>Google</a>
```
 This will open google in a new tab. Without the target, it opens in the current tab itself.

###### Opening mail client
```html
<a href=”mailto:harsh.karaiya@gmail.com” Email me</a>
```

This will open the specified mail id in the users mailing app.

#### Entities
HTML entities are special character codes that represent symbols not easily typed directly in HTML, like special characters, symbols, or reserved HTML characters. 

They begin with an `&` and end with a `;`.

Here’s a list of commonly used HTML entities:

| Entity            | Symbol | Description                    |
|-------------------|--------|--------------------------------|
| `&amp;`           | &      | Ampersand                      |
| `&lt;`            | <      | Less-than symbol               |
| `&gt;`            | >      | Greater-than symbol            |
| `&quot;`          | "      | Double quotation mark          |
| `&apos;`          | '      | Single quotation mark (apostrophe) |
| `&cent;`          | ¢      | Cent sign                      |
| `&euro;`          | €      | Euro sign                      |
| `&pound;`         | £      | British pound sign             |
| `&yen;`           | ¥      | Japanese yen sign              |
| `&copy;`          | ©      | Copyright symbol               |
| `&reg;`           | ®      | Registered trademark symbol    |
| `&trade;`         | ™      | Trademark symbol               |
| `&nbsp;`          |        | Non-breaking space             |
| `&sect;`          | §      | Section symbol                 |
| `&para;`          | ¶      | Paragraph symbol               |
| `&deg;`           | °      | Degree symbol                  |
| `&micro;`         | µ      | Micro symbol                   |
| `&middot;`        | ·      | Middle dot                     |
| `&ndash;`         | –      | En dash                        |
| `&mdash;`         | —      | Em dash                        |
| `&hellip;`        | …      | Ellipsis                       |
| `&raquo;`         | »      | Right double angle quotation mark |
| `&laquo;`         | «      | Left double angle quotation mark |
