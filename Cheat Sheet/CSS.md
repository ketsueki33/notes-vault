##### Hide scrollbar
```css
/* Hide scrollbar for Chrome, Safari and Opera */
.no-scrollbar::-webkit-scrollbar {
    display: none;
}

/* Hide scrollbar for IE, Edge and Firefox */
.no-scrollbar {
    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;  /* Firefox */
}
```

##### @property
This at-rule is used to define a custom property with additional features such as type checking, setting initial values, and controlling inheritance. **It is also used to register custom variables so that it can be used inside @keyframes**

eg) 
```css
@property --rotate {
    syntax: "<angle>";
    initial-value: 0deg;
    inherits: false;
}
```

- **`--rotate`**: 
	- This is the name of the custom property being defined. It's a CSS variable that can be used anywhere in your CSS like any other custom property (e.g., `var(--rotate)`).
- **`syntax: "<angle>";`**:
    
    - This specifies the expected syntax for the custom property value.
    - In this case, `<angle>` indicates that the value for `--rotate` should be a valid CSS angle, such as `0deg`, `45deg`, or `90deg`.
    - This helps with type checking and validation.
- **`initial-value: 0deg;`**:
    
    - This sets the initial value for the `--rotate` property.
    - If no value is specified for `--rotate`, it will default to `0deg`.
- **`inherits: false;`**:
    
    - This determines whether the property should inherit its value from its parent element.
    - `false` means that the custom property `--rotate` will not inherit its value from its parent; instead, it will use the initial value or the value explicitly set on the element.

##### fix Unknown at rule @tailwindcss (unknownAtRules) in VS Code

> [!tip] Fix
> Open a CSS file in your project, and from the VS Code Command Palette choose “Change Language Mode”, then pick “Tailwind CSS” from the list.

##### CSS resets
###### Basic
```css

```
###### Balance headings
```css
h1,h2,h3,h4,h5,h6{
	text-wrap: balance;
}
```

###### Max-width to prevent long stretch of readable text 
```css
p,li,figcaption{
	text-wrap: pretty;
	max-width: 65ch;
}
```

###### Making landmarks as container
```css
header, footer,
main, section, 
article {
	container-type: inline-size;
}
```


