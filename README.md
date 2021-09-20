<img src="https://avatars1.githubusercontent.com/u/3846050?v=4" width="127px" height="127px" align="left"/>

# React Style Guide

Our React projects' best practices

# Introduction

This is meant to be a guide to help new developers understand
the React style and best practices we adopt here at Idealink

---

# Installing a new dependency

To install the package and its dependencies:

```shell
$ npm install --save-dev bootstrap@4.6
```

we **highly recommend** you to make a commit after each installation

# Component definition

All components (presentation, containers or pages) should **always** be
named using the `CamelCase` naming convention. and the files should follow the `camel-case.jsx` naming convention.

# Project organization

Components based on their type should be stored in different directories. the base hierarchy is as follows:

```
public/
└──
    ...
    └── index.html
src/
└── assets/
    ├── images/
    ├── icons/
    ├── fonts/
    └── styles/
        ├── css/
        └── sass/
    ├── components/
        ├── common/
        └── .../
    ├── pages/
    ├── utilities/
    ├── views/
    ├── app.jsx
    └── index.jsx
```

- Styles should always be defined in a separate CSS file, don't put all the css codes all together. (if using sass, we expect to have one single source of styles called `styles.scss` and inside that we only import all the other scss files)
- Any other asset that we might have like videos and similar files, should go to the /assets folder.
- The components like Header, Footer, Sidebar will go to the common folder of our components directory.
- All the components, that are created for only one single page, will be in a folder with the page name inside the /components directory.
- Each page should be inside the /pages directory with the same name as it's route. (also remember that the component must be `CamelCase` and it's file name is like `camel-case.jsx`)
- the format of our files are always `.jsx` for react projects and we don't use the `.js` format
- We put the helper methods, const data (like a very long list) and things like that inside the /utilities directory
- to keep our code clean, we create very small views for some parts and put them inside the /views directory

# Code Standards

### Destruct your `properties`

Properties from an object that are being used in the same place **should** be destructed

### Code style

We use `Prettier - Code formatter` VSCode Extension, with the following configurations:

```json
{
  "tabWidth": 4,
  "useTabs": false,
  "semi": false,
  "printWidth": 100
}
```

#### How To Use
create a `.prettierrc` file, in the root directory of our project, with the above content, after each code change, we restyle the code using `ALT + SHIFT + F` on windows and `COMAND ON MAC` on macOS

### Use Explanatory Variables

Bad

```js
const onlyNumbersRegex = /^\d+$/;
const validateNumber = (message) => (value) =>
  !onlyNumbersRegex.test(value) && message;

validateNumber("error message")(1);
```

Good

```js
const onlyNumbersRegex = /^\d+$/;
const getNumberValidation = (message) => (value) =>
  !onlyNumbersRegex.test(value) && message;

const isNumber = getNumberValidation("error message");

isNumber(1);
```

### Use Searchable Names

Bad

```js
setTimeout(doSomething, 86400000);
```

Good

```js
const DAY_IN_MILLISECONDS = 86400000;

setTimeout(doSomething, DAY_IN_MILLISECONDS);
```

## Naming

- As stated previously, folders and directories must follow the `camel` naming and we appreciate simpe and single word names for our folders and directories.
- We use the `.jsx` format all the time and not the `.js` format
- We use `CamelCase` for the component names and the `camel-case.jsx` for the file names
- We use CSS modules everywhere. CSS modules are great because they provide
scope to CSS and allow us to create compartmentalized styles that don't
leak to global scope.
- use `.dash-case` for css classes all the time.

# React Peculiarities

### Mixins

- [DONT Use Mixins](https://reactjs.org/blog/2016/07/13/mixins-considered-harmful.html)

Why? Mixins introduce implicit dependencies, cause name clashes, and cause snowballing complexity. Most use cases for mixins can be accomplished in better ways via components, higher-order components, or utility modules.

### Never use ID and tag names as root selectors in CSS Modules!

Using ID and tag name at the selector's root makes the rule to be applied
globally. we don't want that.

<table>
<thead>

</tbody>
<th colspan="2"><strong>BAD</strong></th>
</thead>
<tbody>
<tr>
<td>

```js
import style from "./style.css";

const Item = ({ title, thumbnail }) => (
  <div className={style.container}>
    <img src={thumbnail} alt={title} />
  </div>
);

export default Item;
```

</td>
<td>

```css
img {
  background-color: #cccccc;
}
```

</td>
</tr>
<th colspan="2"><strong>GOOD</strong></th>
<tbody>
<tr>
<td>

```js
import style from "./style.css";

const Item = ({ title, thumbnail }) => (
  <div className={style.container}>
    <img src={thumbnail} alt={title} />
  </div>
);

export default Item;
```

</td>
<td>

```css
.container > img {
  background-color: #cccccc;
}
```

</td>
</tr>
</tbody>
</table>

## CSS Design Patterns

### Components never leak margin

All components are self-contained and their final size should never suffer
margin leakage! This allows the components to be much more reusable!

<table>
<thead>
  <th><strong>BAD</strong></th>
  <th><strong>GOOD</strong></th>
</thead>
<tbody>
<tr>
<td>

```
|--|-content size-|--| margin
 ____________________
|   ______________   | | margin
|  |              |  |
|  |              |  |
|  |              |  |
|  |______________|  |
|____________________| | margin

|---container size---|
```

</td>

<td>

```

   |-content size-|
    ______________
   |              |
   |              |
   |              |
   |______________|



```

</td>
</tr>
</tbody>
</table>

### The parent spaces the children

When building lists or grids:

- Build list/grid items as separate components
- Use the the list/grid container to space children
- To space them horizontally, use `margin-left`
- To space them vertically, use `margin-top`
- Select the `first-child` to reset margins

<table>
<thead>
<th colspan="2"><strong>GOOD</strong></th>
</thead>
<tbody>
<tr>
<td>

```js
import style from "./style.css";

const Reviews = ({ items }) => (
  <div className={style.container}>
    {items.map((item) => (
      <img src={item.image} alt={item.title} />
    ))}
  </div>
);

export default Reviews;
```

</td>
<td>

```css
.container > img {
  margin-left: 10px;
}

.container > img:first-child {
  margin-left: unset;
}
```

</td>
</tr>
</tbody>
</table>

### Variables, lots of variables!

We encourage the "variabilification". Always define variables to increase
reuse and make styles more consistent. The CSS4 specification defines a way
to declare native variables. We adopted them as the standard.

To define a variable accessible globally:

<table>
<thead>
<th colspan="2"><strong>GOOD</strong></th>
</thead>
<tbody>
<tr>
<td>

```css
:root {
  --color-green-1: #6ccfae;
  --color-green-2: #6b66b5;
  --color-green-3: #aac257;
  --color-green-4: #68b5c1;
}
```

</td>
<td>

```css
.container {
  background-color: linear-gradient(
    0deg,
    var(--color-green-1),
    var(--color-green-2)
  );
}
```

</td>
</tr>
</tbody>
</table>
