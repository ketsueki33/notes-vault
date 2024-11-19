CSS is comprised of many different layout algorithms, known officially as “layout modes”. Each layout mode is its own little sub-language within CSS. 

The default layout mode is _Flow layout_, but we can opt in to **Flexbox** by changing the `display` property on the parent container:

```css
.parent{
	display: flex;
}
```

When we flip `display` to `flex`, we create a “flex formatting context”. This means that, by default, all children will be positioned according to the Flexbox layout algorithm.

Flexbox is all about arranging a group of items in a row or column, and giving us a _ridiculous_ amount of control over the distribution and alignment of those items. Flexbox is all about _flexibility_. We can control whether items grow or shrink, how the extra space is distributed, and more.

#### Flex direction
As mentioned, Flexbox is all about controlling the distribution of elements in a row or column. By default, items will stack side-by-side in a row, but we can flip to a column with the `flex-direction` property:

```css
.parent{
	display: flex;
	flex-direction: column;
}
```

With `flex-direction: row`, the _primary axis_ runs horizontally, from left to right. When we flip to `flex-direction: column`, the primary axis runs vertically, from top to bottom.

**In Flexbox, everything is based on the primary axis.** The algorithm doesn't care about vertical/horizontal, or even rows/columns. All of the rules are structured around this primary axis, and the _cross axis_ that runs perpendicularly.

The children will be positioned by default according to the following 2 rules:

1. **Primary axis:** Children will be bunched up at the _start_ of the container.
2. **Cross axis:** Children will stretch out to fill the entire container.

![[_assets/flex-direction.png|550]]

In addition to `row` and `column`, there `flex-direction` accepts two more values:
- `row-reverse`: The primary axis runs horizontally but from *right to left*.
- `column-reverse`: The primary axis runs vertically but from *bottom to top*.

#### Alignment
###### justify-content
We can change how children are distributed along the primary axis using the `justify-content` property.

This property is all about the _distribution of the group as a whole_, not individual children.

The possible values for `justify-content` are:

| Value               | Description                                                                                                  |
|---------------------|--------------------------------------------------------------------------------------------------------------|
| `flex-start`        | Aligns items to the start of the container along the main axis (left in a left-to-right layout). (*Default*)    |
| `flex-end`          | Aligns items to the end of the container along the main axis (right in a left-to-right layout).              |
| `center`            | Centers items along the main axis, distributing space evenly on either side of the items.                    |
| `space-between`     | Distributes items with equal space between them, with no space at the start or end of the container.         |
| `space-around`      | Distributes items with equal space around them, giving half-space at the edges and full-space between items. |
| `space-evenly`      | Distributes items with equal space around them, including at the start and end of the container.             |

###### align-items
For aligning children of the flex container along the cross-axis we use the `align-items` property.

This property is all about the *alignment of individual children of the flex container*.

The possible values for `align-items` are:

| Value         | Description                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| `stretch`     | Stretches items to fill the container along the cross-axis (*default*).                                       |
| `flex-start`  | Aligns items to the start of the container along the cross-axis.                                            |
| `flex-end`    | Aligns items to the end of the container along the cross-axis.                                              |
| `center`      | Centers items along the cross-axis.                                                                         |
| `baseline`    | Aligns items along their baseline, which may result in some items positioned higher or lower.               |

###### align-self
Unlike `justify-content` and `align-items`, `align-self` is applied to the _child element_, not the container. 

It allows us to change the alignment of a specific child along the cross axis.

`align-self` has all the same values as `align-items`. In fact, **they change the exact same thing.** `align-items` is _syntactic sugar_, a convenient shorthand that automatically sets the alignment on all the children at once.

###### content vs items

When we are talking about alignment in the _cross_ axis, each item can do whatever it wants. In the _primary_ axis, though, we can only think about how to distribute the _group_ as a single item can't move along the primary axis without disturbing or overlapping with it's siblings.

**That's why there's no** `justify-self`**.** What would it mean for that middle piece to set `justify-self: flex-start`? There's already another piece there!

With all of this context in mind, let's give a proper definition to all 4 terms we've been talking about:
- `justify` — to position something along the _primary axis_.
- `align` — to position something along the _cross axis_.
- `content` — a group of “stuff” that can be distributed.
- `items` — single items that can be positioned individually.

**And so:** we have `justify-content` to control the distribution of the group along the primary axis, and we have `align-items` to position each item individually along the cross axis. These are the two main properties we use to manage layout with Flexbox.

There's no `justify-items` for the same reason that there's no `justify-self`; when it comes to the primary axis, _we have to think of the items as a group,_ as content that can be distributed.

#### Hypothetical Size
Setting the `width` property of a normal element and a element that is the child of a flex container yields different results.

The normal element will be rendered using Flow layout, and in Flow layout, `width` is a _hard constraint_. When we set `width: 2000px`, we'll get a 2000-pixel wide element, even if it has to overflow outside it's container or even the viewport.

In _Flexbox_, however, the `width` property is implemented differently. It's more of a suggestion than a hard constraint.

The specification has a name for this: the **hypothetical size**. It's the size an element _would_ be, in a perfect scenario, with nothing getting in the way.

So, if the parent container _doesn't have room_ for a 2000px-wide child, it's size is reduced so that it fits.

This is a core part of the Flexbox layout. Things are fluid and flexible and can adjust to the constraints of the container.

#### Growing and Shrinking
We've now seen that the Flexbox algorithm has some built-in flexibility, with _hypothetical sizes_. But to _really_ understand how fluid Flexbox can be, we need to look at these 3 properties: `flex-grow`, `flex-shrink`, and `flex-basis`.

###### flex-basis
**To put it in simple terms:** In a Flex row, `flex-basis` does the same thing as `width`. In a Flex column, `flex-basis` does the same thing as `height`.

As we've learned, everything in Flexbox is _depends on the primary/cross axis_. For example, `justify-content` will distribute the children along the primary axis, and it works exactly the same way whether the primary axis runs horizontally or vertically.

`width` and `height` don't follow this rule, though! `width` will always affect the horizontal size. It doesn't suddenly become `height` when we flip `flex-direction` from `row` to `column`.

**So, the Flexbox has a generic “size” property called `flex-basis`.** It's like `width` or `height`, but pegged to the _primary axis_, like everything else. It allows us to set the _hypothetical size_ of an element in the primary-axis direction, regardless of whether that's horizontal or vertical.

Like we saw with `width`, `flex-basis` is *more of a suggestion than a hard constraint*. If there isn't enough space for all of the elements to sit at their assigned size, they have to compromise, in order to avoid an overflow.


> [!warning] width and flex-basis not exactly the same
> In general, we can use `width` and `flex-basis` interchangeably in a Flex row, but there are some exceptions. For example, the `width` property affects replaced elements like images differently than `flex-basis`. Also, `width` can reduce an item below its _minimum_ size, while `flex-basis` can't.

###### flex-grow
By default, elements in a Flex context will shrink down to their minimum comfortable size along the primary axis. This often creates extra space.

We can specify how that space should be consumed with the `flex-grow` property.

The *default value for `flex-grow` is 0*, which means that growing is opt-in. If we want a child to gobble up any extra space in the container, we need to explicitly tell it so.

**`flex-grow` on a single child**:
When a single child is given a positive `flex-grow` value, it gobbles up all of the extra space. In this case, it doesn't matter what the number is: 1 and 1000 have the same effect.

**`flex-grow` on multiple children**:
In this case, the extra space is divided between children, proportionally based on their `flex-grow` value.

*Examples:*
- 2 children with `flex-grow` = 1 , 3 : 
	- The first child wants 1 unit of extra space, while the second child wants 3 units. That means the total # of units is **4** (1 + 3). First child gets $1/4$ of total extra space while the second one gets $3/4$ of the total extra space.
- 3 children with `flex-grow` = 2, 2, 4
	- The first child wants 2 units of extra space, the second child also wants 2 units, and the third child wants 4 units. That means the total number of units is **8** (2 + 2 + 4).
	- The first child gets $\frac{2}{8} = \frac{1}{4}$ (or 25%) of the total extra space.
	- The second child also gets $\frac{2}{8} = \frac{1}{4}$ (or 25%) of the total extra space.
	- The third child receives $\frac{4}{8} = \frac{1}{2}$ (or 50%) of the total extra space.


###### flex-shrink
We know how flexbox deals with extra space.. but what if our flexbox container is not big enough for it's children?

Each child will shrink and they will shrink *proportionally*.

`flex-basis` and `width` set the elements' _hypothetical size_. The Flexbox algorithm might shrink elements below this desired size, but by default, they'll always scale together, preserving the ratio between both elements.

We **use the `flex-shrink` property to control** the shrinking of our elements. The *default value for `flex-shrink` is 1*. It determines the factor by which that element will shrink relative to its flexbox siblings.

Think of it like a factor by which the total size deficit is paid, while trying to preserve the proportions between siblings.

*Example:*
Suppose we have 2 children with `flex-shrink` = 3 , 1 ( with size deficit of 100px):

We have a total deficit of _100px_. Normally, each child would pay ½, but because we've set the `flex-shrink` of first child as 3, the first element winds up paying ¾ (75px), and the second element pays ¼ (25px).

![[_assets/flex-shrink.png|400]]

Note that the absolute values don't matter, **it's all about the ratio.** If both children have `flex-shrink: 1`, each child will pay ½ of the total deficit. If both children are cranked up to `flex-shrink: 1000`, each child will pay 1000/2000 of the total deficit. Either way, it works out to the same thing.

**Preventing shrinking**
Sometimes, we don't _want_ some of our Flex children to shrink.

We can do this by setting `flex-shrink: 0`

When we set `flex-shrink` to 0, **we essentially “opt out” of the shrinking process altogether.** The Flexbox algorithm will treat `flex-basis` (or `width`) as a hard minimum limit.

###### flex shorthand
The `flex` shorthand in CSS combines three flex properties into a single, concise declaration:
```css
flex: <flex-grow> <flex-shrink> <flex-basis>;
```


#### Flexbox and Minimum Size
In addition to the _hypothetical_ size, there's another important size that the Flexbox algorithm cares about: _the minimum size_.

The Flexbox algorithm refuses to shrink a child below its minimum size. The content will overflow rather than shrink further, _no matter how high we crank `flex-shrink`!_

Some text inputs have a default minimum size of 170px-200px (it varies between browsers). For an element containing text, the minimum width is the length of the _longest unbreakable string of characters._ These will not shrink beyond that in a flexbox.

To override this behaviour, we have to set `min-width` of those items to `0px`. 

By setting `min-width: 0px` directly on the Flex child, the element can shrink as much as necessary.


> [!warning] min-width overriding
> It's worth noting that the built-in minimum size _does_ serve a purpose. It's meant to act as a guardrail, to prevent something even worse from happening.
> 
> For example: when we apply `min-width: 0px` to our text-containing Flex children, things can break in an even worse way (when shrinking).

#### Gaps
The `gap` property allows us to create space _in-between_ each Flex child. It accepts any unit of length and percentage.

##### Auto margins
The `margin` property is used to add space around a specific element. In some layout modes, like Flow and Positioned, it can even be used to center an element, with `margin: auto`.

Auto margins are much more interesting in Flexbox.

Earlier, we saw how the `flex-grow` property can gobble up any extra space, applying it to a child.

**Auto margins will gobble up the extra space, and apply it to the element's margin.** It gives us precise control over where to distribute the extra space.

A common header layout features the logo on one side, and some navigation links on the other side. Here's how we can build this layout using auto margins:
```html
<style>
  ul {
    display: flex;
    gap: 12px;
  }
  li.logo {
    margin-right: auto;
  }
</style>

<nav>
  <ul>
    <li class="logo">
      <a href="/">
        Corpatech
      </a>
    </li>
    <li>
      <a href="">
        Mission
      </a>
    </li>
    <li>
      <a href="">
        Contact
      </a>
    </li>
  </ul>
</nav>
```

The **Corpatech** logo is the first list item in the list. By giving it `margin-right: auto`, we gather up all of the extra space, and force it between the 1st and 2nd item.

We can see what's going on here using the browser devtools:

![[_assets/auto-margin-flexbox.png|400]]

#### Wrapping
The `flex-wrap` property sets whether flex items are forced onto one line or can wrap onto multiple lines. If wrapping is allowed, it sets the direction that lines are stacked.

When we set `flex-wrap: wrap`, **items won't shrink below their hypothetical size**.

With `flex-wrap: wrap`, we can have multiple primary axis lines. Effectively, **each row acts as its own mini flex container.**

All of the rules we have learnt for aligning will continue to apply. 

`justify-content` will distribute the elements on primary axes.

Since each row is its own mini Flexbox environment. `align-items` will move each item up or down within the invisible box that wraps around each row.

But what if we want to _align the rows themselves_? We can do that with the `align-content` property. The possible values for this property are:

| Value             | Description                                                                                           |
|-------------------|-------------------------------------------------------------------------------------------------------|
| `stretch`         |  Stretches content(mini flexboxes) to fill the container along the cross axis.(*Default*)                                  |
| `flex-start`      | Packs content to the start of the cross axis (top if the flex direction is row).                        |
| `flex-end`        | Packs content to the end of the cross axis (bottom if the flex direction is row).                       |
| `center`          | Centers content along the cross axis.                                                                   |
| `space-between`   | Distributes content evenly with the first item at the start and the last item at the end.               |
| `space-around`    | Distributes content with equal space around each item.                                                  |
| `space-evenly`    | Distributes content with equal space between, before, and after each item.                              |


#### Responsive layout with Flexbox

Flexbox is very good for creating dynamic layouts that are responsive, rearranging themselves as needed.

For example consider this form:
```html title="Flex Form"
<style>
  form {
    display: flex;
    align-items: flex-end;
    flex-wrap: wrap;
    gap: 8px;
  }
  .name {
    flex-grow: 1;
    flex-basis: 120px;
  }
  .email {
    flex-grow: 3;
    flex-basis: 170px;
  }
  button {
    flex-grow: 1;
    flex-basis: 70px;
  }
</style>

<form>
  <label class="name" for="name-field">
    Name:
    <input id="name-field" />
  </label>
  <label class="email" for="email-field">
    Email:
    <input id="email-field" type="email" />
  </label>
  <button>
    Submit
  </button>
</form>
```

This single flexbox container can arrange in 4 different ways depending on the container width:

![[_assets/responsive-flexbox-1.png|500]]

![[_assets/responsive-flexbox-2.png|480]]

![[_assets/responsive-flexbox-3.png|390]]

![[_assets/responsive-flexbox-4.png|310]]

This form can rearrange in different ways due to the combination of `flex-grow`, `flex-basis`, and `flex-wrap` properties.