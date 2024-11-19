While Flexbox is designed for distributing items along a single dimension, Grid Layout allows us to distribute items flexibly along two dimensions.

The grid _structure_, the rows and columns, are defined **purely in CSS:** Unlike Table layout, CSS Grid lets us manage the layout entirely from within CSS. We can slice up the container however we wish, creating compartments that our grid children can use as anchors.

#### Grid Terminology
**Grid Container**:  
This is the element with `display: grid` applied, making it the parent of grid items. The grid container defines the boundaries of the grid, allowing you to position and align child elements on a grid layout.

**Grid Items**:  
These are the direct child elements of a grid container, automatically laid out based on the grid's structure. Each item occupies a cell (or multiple cells) in the grid, and you can control their position with properties like `grid-column` and `grid-row`.

**Grid Gaps**:  
This defines the space between grid items. You can set `gap` (or `row-gap` and `column-gap` individually) to control spacing without extra margins or padding on items.

**Grid Lines**: 
The horizontal and vertical lines that separate the grid tracks (rows and columns). They create the boundaries of each grid cell. Grid lines are numbered, which allows you to position and span items across specific lines.

**Grid Tracks**: 
The rows and columns themselves within the grid. You define their sizes using `grid-template-rows` and `grid-template-columns`.

**Grid Cells**: 
The individual rectangular areas between four intersecting grid lines. A grid item will usually occupy a single grid cell unless it spans across multiple cells.

#### Grid flow
We opt in to the Grid layout mode with the `display` property:

```css
.wrapper {
  display: grid;
}
```

By default, CSS Grid uses a single column, and will create rows as needed, based on the number of children. This is known as an _implicit grid_, since we aren't explicitly defining any structure.

Implicit grids are dynamic; rows will be added and removed based on the number of children. Each child gets its own row.

By default, the height of the grid parent is determined by its children. It grows and shrinks dynamically. Interestingly, this isn't even a “CSS Grid” thing; the grid _parent_ is still using Flow layout, and block elements in Flow layout grow vertically to contain their content. Only the _children_ are arranged using Grid layout.

If we give the grid a fixed height, the total surface area is divided into equally-sized rows.

#### Grid Construction
By default, CSS Grid will create a single-column layout. We can specify columns using the `grid-template-columns` property:
```html hl=4
<style>
  .parent {
    display: grid;
    grid-template-columns: 25% 75%;
  }
</style>

<div class="parent">
  <div class="child">1</div>
  <div class="child">2</div>
</div>
```

By passing two values to `grid-template-columns` — `25%` and `75%` — we are telling the CSS Grid algorithm to slice the parent container up into two columns.

Columns can be defined using any valid [CSS `<length-percentage>` value](https://developer.mozilla.org/en-US/docs/Web/CSS/length-percentage), including pixels, rems, viewport units, and so on. Additionally, we also gain access to a new unit, the `fr` unit:

> [!info]+ the 'fr' unit
> `fr` stands for “fraction”. The `fr` unit brings Flexbox-style flexibility to CSS Grid. Percentages and `<length>` values create hard constraints, while `fr` columns are free to grow and shrink as required, to contain their contents.
> 
> To be more precise: the `fr` unit distributes _extra_ space. First, column widths will be calculated based on their contents. If there's any leftover space, it'll be distributed based on the `fr` values. This is very similar to `flex-grow`.
> ```html hl=4
> <style>
>   .parent {
>     display: grid;
>     grid-template-columns: 1fr 3fr;
>   }
> </style>
> 
> <div class="parent">
>   <div class="child">1</div>
>   <div class="child">2</div>
> </div>
> ```
> In this example, we're saying that the first column should consume 1 unit of space, while the second column consumes 3 units of space. That means there are 4 total units of space, and this becomes the denominator. The first column eats up ¼ of the available space, while the second column consumes ¾.

In general, this flexibility is a good thing. Percentages are too strict. If we try to to use the `gap` property of the grid when using percentage-based columns, the contents may spill outside the grid container. 

This happens because percentages are calculated using the _total_ grid area. The two columns consume 100% of the parent's content area, and they aren't allowed to shrink. When we add 16px of `gap`, the columns have no choice but to spill beyond the container.

The `fr` unit, by contrast, is calculated based on the _extra_ space. In this case, the extra space has been reduced by 16px, for the `gap`. The CSS Grid algorithm distributes the remaining space between the two grid columns.


##### Implicit and explicit rows
If we add more than two children to a two-column grid, the grid will gain extra rows. The grid algorithm wants to ensure that every child has its own grid cell. It’ll spawn new rows as-needed to fulfill this goal.

In other situations, though, we want to define the rows explicitly, to create a specific layout. We can do that with the `grid-template-rows` property:
```css
.parent {
    display: grid;
    grid-template-columns: 1fr 3fr;
    grid-template-rows: 5rem 1fr;
}
```

By defining both `grid-template-rows` and `grid-template-columns`, we've created an explicit grid. This is perfect for building page layouts.

We can use `grid-auto-rows` & `grid-auto-columns` to style implicitly created rows & columns.

> [!NOTE]+ Implicit Columns
> Implicit columns are less common than implicit rows. One way to create them is to define more `grid-template-areas` than  `grid-template-columns`.
> ```css
> .grid-container {
>   display: grid;
>   grid-template-areas: "media detail detail";
>   grid-template-columns: 200px 400px;
>   grid-auto-columns: 150px;
> }
> ```
> 
> This example creates a grid container with three columns defined by `grid-template-areas`. The first two columns have an explicit size set by `grid-template-columns`, but not the last column. This is where `grid-auto-columns` kicks in and sets that column’s size to `150px`.

##### grid-auto-flow
The `grid-auto-flow` property in CSS controls how the grid layout algorithm places items into the grid when there are not enough defined grid cells or tracks to accommodate them. It specifies the direction (row or column) and the flow order for automatically placed items within a CSS grid.

**Values**:

1. `row` *(default)*:
    
    - Places items in rows by default.
    - If a row is full, it moves to the next row.
2. **`column`**:
    
    - Places items in columns.
    - Fills one column from top to bottom and then moves to the next column.
3. **`dense`**:
    
    - This option allows items to backfill any gaps in the grid layout.
    - The layout will try to place items in available smaller spaces within the grid, regardless of order.
4. **Combinations (`row dense`, `column dense`)**:
    
    - For example, `row dense` places items in rows and backfills gaps as needed.
    - `column dense` places items in columns and backfills any gaps created.

##### the repeat helper
Let's suppose we're building a calendar where each day is a column. 

CSS Grid is a wonderful tool for this sort of thing. We can structure it as a 7-column grid, with each column consuming 1 unit of space: 
```css
.calendar {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
}
```

But it's annoying to write the same thing so many times. Instead we can do this:
```css
.calendar {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}
```

The `repeat` function will do the copy/pasting for us. We're saying we want 7 columns that are each `1fr` wide.

##### Automatic number of columns
There is a way to make our grid responsive ( changing number of columns ) without using media or container queries. We will be using `repeat` , `auto-fit` and `minmax` to achieve this.
```css hl=3
.container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
}
```

`auto-fit` tells `repeat` the number of columns needed to fit cells of the specified size.

**`minmax(250px, 1fr)`**: This function sets a minimum and maximum size for each column:
- **`250px`** (minimum width): Ensures each column is at least 250 pixels wide.
- **`1fr`** (maximum width): Allows columns to take up one fraction of the remaining space, growing as needed to fill the container.

#### Assigning Grid cells
By default, the CSS Grid algorithm will assign each child to the first unoccupied grid cell.

Cool thing about CSS Grid is that we can assign our items to whichever cells we want! Children can even span across multiple rows/columns.

The `grid-row` and `grid-column` properties allow us to specify which track(s) our grid child should occupy.

If we want the child to occupy a single row or column, we can specify it by its number. `grid-column: 3` will set the child to sit in the third column.

Grid children can also stretch across multiple rows/columns. The syntax for this uses a slash to delineate start and end:
```css
.child {
  grid-column: 1 / 4;
}
```

At first glance, this looks like a fraction, ¼. In CSS, though, the slash character is not used for division, it's used to separate groups of values. In this case, it allows us to set the start and end columns in a single declaration.

It's essentially a shorthand for this:
```css
.child {
  grid-column-start: 1;
  grid-column-end: 4;
}
```

**The important thing to note is:** The numbers we're providing are based on the column _lines_(or grid lines), not the column indexes. For example:
```css
.child {
  grid-column: 1 / 4;
  grid-row: 2 / 4;
}
```
![[_assets/gird-placement.png|420]]


> [!info] Negative line numbers
> In a left-to-right language like English, we count the columns from left to right. With negative line numbers, however, we can also count in the _opposite_ direction, from right to left.
> 
> ```css
> .child {
>   /* Sit in the 2nd column from the right: */
>   grid-column: -2;
> }
> ```
> 
> We can also mix positive and negative numbers.
> ```css
> .child {
>   /* Occupy all the cells in a row */
>   grid-column: 1 / -1;
>   grid-row: 2;
> }
> ```


##### Grid Areas
Let's suppose we're building this layout:

![[_assets/grid-areas.png|350]]

It is possible to structure it like this:
```css
.grid {
  display: grid;
  grid-template-columns: 2fr 5fr;
  grid-template-rows: 50px 1fr;
}

.sidebar {
  grid-column: 1;
  grid-row: 1 / 3;
}
.header {
  grid-column: 2;
  grid-row: 1;
}
.main {
  grid-column: 2;
  grid-row: 2;
}
```

But there's a more ergonomic way to this: **grid areas**:
```css
.parent {
  display: grid;
  grid-template-columns: 2fr 5fr;
  grid-template-rows: 50px 1fr;
  grid-template-areas:
    'sidebar header'
    'sidebar main';
}

.sidebar {
  grid-area: sidebar;
}

.header {
  grid-area: header;
}

.main {
  grid-area: main;
}
```

Areas work best when the grid has a fixed number of rows and columns. `grid-column` and `grid-row` can be useful for implicit grids.

#### Alignment
##### Aligning Columns
We can control the distribution of the columns using the `justify-content` property (as long as the grid parent is larger than the total width of all columns):
```css hl=4
.parent {
  display: grid;
  grid-template-columns: 90px 90px;
  justify-content: space-around;
}
```

CSS Grid builds on the alignment properties first introduced with Flexbox, taking them even further. 

If you're familiar with the Flexbox layout algorithm, this probably feels pretty familiar. CSS Grid builds on the alignment properties first introduced with Flexbox, taking them even further.

**The big difference is that we're aligning the _columns_, not the items themselves.** Essentially, `justify-content` lets us arrange the compartments of our grid, distributing them across the grid however we wish.

If we want to align the items themselves _within_ their columns, we can use the `justify-items` property:
```css hl=5
.parent {
  display: grid;
  grid-template-columns: 90px 90px;
  justify-content: start;
  justify-items: end;
}
```

The default behaviour of a grid item is to stretch across that entire column.With `justify-items`, however, we can tweak that behaviour.

This is useful because it allows us to break free from the rigid symmetry of columns. When we set `justify-items` to something other than `stretch`, the children will shrink down to their default width, as determined by their contents. As a result, items in the same column can be different widths.

We can even control the alignment of a _specific_ grid child using the `justify-self` property:
```css hl=8
.parent {
  display: grid;
  grid-template-columns: 90px 90px;
  justify-content: start;
}

.one {
  justify-self: center;
}
```

Unlike `justify-items`, which is set on the grid parent and controls the alignment of _all_ grid children, `justify-self` is set on the child. We can think of `justify-items` as a way to set a default value for `justify-self` on all grid children.

##### Aligning Rows
So far, we've been talking about how to align stuff in the _horizontal_ direction. CSS Grid provides an additional set of properties to align stuff in the _vertical_ direction:

`align-content` is like `justify-content`, but it affects rows instead of columns.

Similarly, `align-items` is like `justify-items`, but it handles the _vertical_ alignment of items inside their grid area, rather than horizontal.

To break things down even further:
- `justify` — deals with _columns_.
- `align` — deals with _rows_.
- `content` — deals with the _grid structure_. 
- `items` — deals with the _DOM nodes_ within the grid structure.

Finally, in addition to `justify-self`, we also have `align-self`. This property controls the vertical position of a single grid item within its cell.

##### Two-line centering trick
Using only two CSS properties, we can center a child within a container, both horizontally and vertically:
```css
.parent {
  display: grid;
  place-content: center;
}
```

The `place-content` property is a shorthand. It's syntactic sugar for this:
```css
.parent {
  justify-content: center;
  align-content: center;
}
```