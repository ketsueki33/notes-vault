The `position` property specifies the type of positioning method used for an element.

There are five different position values:

- `static`
- `relative`
- `fixed`
- `absolute`
- `sticky`

Elements are then positioned using the `top`, `bottom`, `left`, `right` and `z-index` properties. However, these properties will not work unless the `position` property is set first. They also work differently depending on the position value.

#### position values
##### position: static;
The element is positioned according to the Normal Flow of the document. The `top`, `right`, `bottom`, `left`, and `z-index` properties have no effect.

This is the **default** value.

##### position: relative;
The element is positioned according to the normal flow of the document, and then offset _relative to itself_ based on the values of `top`, `right`, `bottom`, and `left`. 

The offset does not affect the position of any other elements; thus, the space given for the element in the page layout is the same as if position were `static`. (Other content will not be adjusted to fit into any gap left by the element.)

This value creates a new [[#Stacking Context|stacking context]] when the value of `z-index` is not `auto`.

##### position: absolute;
The element is removed from the normal document flow, and no space is created for the element in the page layout. 

The element is positioned relative to its closest positioned ancestor (if any) or to the initial containing block. Its final position is determined by the values of `top`, `right`, `bottom`, and `left`.

This value creates a new [[#Stacking Context|stacking context]] when the value of `z-index` is not `auto`.

##### position: fixed;
The element is removed from the normal document flow, and no space is created for the element in the page layout.

An element with `position: fixed;` is positioned relative to the viewport, which means it always stays in the same place even if the page is scrolled. The top, right, bottom, and left properties are used to position the element.

This value always creates a new [[#Stacking Context|stacking context]].

##### position: sticky;
The element is positioned according to the normal flow of the document until a given offset position is met in the viewport - then it "sticks" in place (like position:fixed), based on the values of `top`, `right`, `bottom`, and `left`. The offset does not affect the position of any other elements.

This value always creates a new [[#Stacking Context|stacking context]]. Note that a sticky element "sticks" to its nearest ancestor that has a "scrolling mechanism" (created when `overflow` is `hidden`, `scroll`, `auto`, or `overlay`), even if that ancestor isn't the nearest actually scrolling ancestor.


#### Stacking Context
**Stacking context** is a three-dimensional conceptualization of HTML elements along an imaginary z-axis relative to the user, who is assumed to be facing the viewport or the webpage. HTML elements occupy this space in priority order based on element attributes.