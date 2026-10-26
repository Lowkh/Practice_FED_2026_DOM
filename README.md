# Understanding the Three Files

This project uses three files that work together:

```text
index.html  → Creates the page
style.css   → Styles the page
app.js      → Makes the page interactive
```

![Diagram showing how HTML creates the page, JavaScript decides what happens, and CSS displays the result.](pics/html-css-js.png)

Think of the project like a shop:

| File | Shop comparison | Job in this project |
|---|---|---|
| HTML | The shop building and items | Creates the search form and activity cards. |
| CSS | The colours, layout, and signs | Makes the webpage easy to read and hides cards when needed. |
| JavaScript | The shop assistant | Responds to users and decides which activities to show. |

---

## HTML: The Page Content

HTML creates the things that a user can see and use.

In this project, HTML creates:

- The page title.
- The search input box.
- The category dropdown menu.
- The Search and Clear buttons.
- The status message.
- The activity cards.

Example:

```html
<input id="keyword" type="text">
```

This creates a text input box.

The `id` gives the input box a unique name:

```text
keyword
```

JavaScript will use that name later to find the input box.

```html
<article class="activity-card sports">
  <h2>Basketball Club</h2>
  <p>Play weekly basketball games with other students.</p>
</article>
```

This creates one activity card.

It has two classes:

| Class | Meaning |
|---|---|
| `activity-card` | This item is an activity card. |
| `sports` | This activity belongs to the Sports category. |

### HTML Remember

> HTML creates the page content and gives JavaScript targets to find.

---

## CSS: The Page Appearance

CSS controls how the HTML looks.

In this project, CSS controls:

- Font style and text size.
- Page spacing.
- Card colour and layout.
- The appearance of buttons and form controls.
- Whether a card is visible or hidden.

Example:

```css
.activity-card {
  padding: 16px;
  background: white;
  border-left: 5px solid #2563eb;
}
```

This CSS makes an activity card look like a card.

The most important CSS rule is:

```css
.is-hidden {
  display: none;
}
```

This means:

> If an element has the class `is-hidden`, do not show it on the page.

For example, this card is visible:

```html
<article class="activity-card sports">
```

This card is hidden:

```html
<article class="activity-card sports is-hidden">
```

### CSS Remember

> CSS controls how the page looks, including visual states such as visible and hidden.

---

## JavaScript: The Page Behaviour

JavaScript makes the webpage respond to the user.

In this project, JavaScript:

- Finds HTML elements.
- Waits for user actions.
- Reads the search word and selected category.
- Checks every activity card.
- Decides whether each card matches.
- Adds or removes the `is-hidden` class.
- Updates the status message.

Example:

```js
const keywordInput = document.querySelector("#keyword");
```

This tells JavaScript:

> Find the HTML element with the id `keyword`.

Example:

```js
clearButton.addEventListener("click", function () {
  keywordInput.value = "";
});
```

This tells JavaScript:

> When the Clear button is clicked, empty the search input box.

Example:

```js
card.classList.toggle("is-hidden", !shouldShow);
```

This tells JavaScript:

> Hide the card if it should not be shown. Otherwise, show it.

### JavaScript Remember

> JavaScript listens, reads, decides, and updates the page.

---

## How the Files Work Together

Consider this user action:

> The user searches for `robot` and chooses Technology.

### 1. HTML provides the elements

HTML contains:

```html
<input id="keyword">
```

```html
<select id="category">
```

```html
<article class="activity-card technology">
```

### 2. JavaScript reads the user choices

JavaScript reads:

```js
const query = keywordInput.value.trim();
```

```js
const category = categorySelect.value;
```

The values are:

```text
query = "robot"
category = "technology"
```

### 3. JavaScript decides which card matches

JavaScript checks each activity card.

For Robotics Challenge:

```text
Does the text contain "robot"? Yes.
Does the card have class "technology"? Yes.
```

Therefore, the card should be shown.

For Coding Workshop:

```text
Does the text contain "robot"? No.
Does the card have class "technology"? Yes.
```

Therefore, the card should be hidden.

### 4. JavaScript changes the CSS class

JavaScript adds `is-hidden` to cards that do not match:

```js
card.classList.toggle("is-hidden", !shouldShow);
```

### 5. CSS displays the result

CSS hides cards with this rule:

```css
.is-hidden {
  display: none;
}
```

The result is that only Robotics Challenge remains visible.

---

## Simple Summary

| Question | Answer |
|---|---|
| Which file creates the input box? | `index.html` |
| Which file makes the card look like a card? | `style.css` |
| Which file checks whether `robot` matches a card? | `app.js` |
| Which file hides a card with `display: none`? | `style.css` |
| Which file adds `is-hidden` to a card? | `app.js` |

> HTML creates. CSS styles. JavaScript responds.
