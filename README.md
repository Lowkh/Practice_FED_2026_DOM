# Activity Directory

A simple beginner project for learning how JavaScript makes a webpage respond to user actions.

---

## What You Will Build

You will create a webpage that allows a user to:

- Search activities using a keyword.
- Filter activities by category.
- Clear all filters.
- See how many activity cards are visible.

For example:

1. Type `robot`.
2. Choose `Technology`.
3. Only **Robotics Challenge** should remain visible.

---

## HTML, CSS, and JavaScript

A webpage uses three languages that work together:

- **HTML** creates the content.
- **CSS** controls how the content looks.
- **JavaScript** responds when the user interacts with the page.

![Diagram showing how HTML creates the page, JavaScript decides what happens, and CSS displays the result.](pics/html-css-js.png)

| Language | Simple explanation |
|---|---|
| HTML | Creates the search box, dropdown menu, buttons, and activity cards. |
| CSS | Styles the page and hides cards when necessary. |
| JavaScript | Reads the user’s choices and decides which cards should be visible. |

---

## How the App Works

Every interaction follows the same process:

![Diagram showing the seven DOM interaction steps: Select, Iterate, Listen, Read, Decide, Update, and Check.](pics/dom-steps.png)

1. **Select** — JavaScript finds the form, inputs, buttons, message, and activity cards.
2. **Iterate** — JavaScript checks one activity card at a time.
3. **Listen** — JavaScript waits for the user to do something.
4. **Read** — JavaScript reads the search word and category.
5. **Decide** — JavaScript checks whether a card matches.
6. **Update** — JavaScript shows or hides cards.
7. **Check** — JavaScript updates the number of visible cards.

If something does not work, check these steps in order. The first step that fails is usually where the problem is. [file:1]

---

## Project Files

```text
activity-directory/
├── index.html
├── style.css
├── app.js
├── README.md
└── pics/
    ├── dom-steps.png
    ├── html-css-js.png
    ├── selectors.png
    └── events-values.png
```

| File or folder | Purpose |
|---|---|
| `index.html` | Creates the webpage content. |
| `style.css` | Styles the webpage. |
| `app.js` | Adds interaction and filtering. |
| `pics/` | Stores learning diagrams used in this README. |

---

# Step 1: Create the HTML

Open `index.html` and add the HTML code for the form and activity cards.

## Important HTML names

An `id` is a unique name for one HTML element.

```html
<input id="keyword">
```

A `class` is a label that can be used on many elements.

```html
<article class="activity-card sports">
```

The image below shows the difference between selecting one element and selecting many elements.

![Diagram comparing querySelector for one element and querySelectorAll for multiple activity cards.](pics/selectors.png)

| HTML | JavaScript selector |
|---|---|
| `id="keyword"` | `"#keyword"` |
| `class="activity-card"` | `".activity-card"` |

Use `defer` when connecting `app.js`:

```html
<script src="app.js" defer></script>
```

`defer` makes JavaScript wait until HTML has loaded. This prevents JavaScript from looking for elements before they exist. [file:1]

---

# Step 2: Select HTML Elements

JavaScript needs to find the HTML elements before it can use them.

```js
const form = document.querySelector("#search-form");
const keywordInput = document.querySelector("#keyword");
const categorySelect = document.querySelector("#category");
const clearButton = document.querySelector("#clear-button");
const status = document.querySelector("#status");

const cards = document.querySelectorAll(".activity-card");
```

Use:

```js
document.querySelector()
```

when you need one element.

Use:

```js
document.querySelectorAll()
```

when you need many elements.

Check that JavaScript found the elements:

```js
console.log(form);
console.log(keywordInput);
console.log(cards.length);
```

Expected result:

- `form` and `keywordInput` should not be `null`.
- `cards.length` should match the number of activity cards.

---

# Step 3: Listen for User Actions

JavaScript waits for user actions called **events**.

![Diagram showing the events used by the search input, category dropdown, Search button, and Clear button.](pics/events-values.png)

| User action | Event | Why it is used |
|---|---|---|
| User types in a text field | `input` | Useful for optional live search. |
| User chooses a category | `change` | Runs when the selected option changes. |
| User submits the search form | `submit` | Runs when Search is clicked or Enter is pressed. |
| User clicks Clear | `click` | Runs when the Clear button is pressed. |

Read the values like this:

```js
const query = keywordInput.value.trim();
const category = categorySelect.value;
```

- `.value` reads the text input or selected dropdown value.
- `.trim()` removes extra spaces before and after a search word. [file:1]

---

# Step 4: Check Every Activity Card

`querySelectorAll()` gives JavaScript a collection of cards.

Use `forEach()` to check one card at a time:

```js
cards.forEach(function (card) {
  console.log(card.textContent);
});
```

This means:

> For every activity card, run the code inside the braces.

`card` represents one current activity card.

---

# Step 5: Decide Which Cards Match

Create a function to filter the cards.

```js
function filterActivities(query, category) {
  let visibleCount = 0;

  cards.forEach(function (card) {
    const cardText = card.textContent.toLowerCase();

    const matchesKeyword = cardText.includes(query.toLowerCase());

    const matchesCategory =
      category === "all" || card.classList.contains(category);

    const shouldShow = matchesKeyword && matchesCategory;

    card.classList.toggle("is-hidden", !shouldShow);

    if (shouldShow) {
      visibleCount = visibleCount + 1;
    }
  });

  status.textContent = visibleCount + " activities found";
}
```

The function checks two things:

1. Does the card contain the search word?
2. Does the card match the selected category?

```js
const shouldShow = matchesKeyword && matchesCategory;
```

The `&&` symbol means **AND**.

A card appears only when both answers are true.

| Keyword matches | Category matches | Show card? |
|---|---|---|
| Yes | Yes | Yes |
| Yes | No | No |
| No | Yes | No |
| No | No | No |

---

# Step 6: Hide or Show Cards

CSS controls what a hidden card looks like:

```css
.is-hidden {
  display: none;
}
```

JavaScript adds or removes the CSS class:

```js
card.classList.toggle("is-hidden", !shouldShow);
```

- If `shouldShow` is true, the card stays visible.
- If `shouldShow` is false, JavaScript adds `is-hidden`.
- CSS then hides the card. [file:1]

---

# Step 7: Connect the Search Form

Use the `submit` event for the form:

```js
form.addEventListener("submit", function (event) {
  event.preventDefault();

  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

`event.preventDefault()` stops the browser from reloading the page after the form is submitted.

---

# Step 8: Connect the Category Menu

Use the `change` event for the dropdown menu:

```js
categorySelect.addEventListener("change", function () {
  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

---

# Step 9: Connect the Clear Button

Use the `click` event for the Clear button:

```js
clearButton.addEventListener("click", function () {
  keywordInput.value = "";
  categorySelect.value = "all";

  filterActivities("", "all");
});
```

This code:

1. Removes the search word.
2. Changes the dropdown back to All categories.
3. Shows all activity cards again.

---

# Test Your Work

| Test | Action | Expected result |
|---|---|---|
| Show all | Leave the search field empty and choose All categories | All cards appear. |
| Search | Search for `coding` | Only Coding Workshop appears. |
| No result | Search for `swimming` | No cards appear and the status says `0 activities found`. |
| Category | Choose Sports | Only sports activity cards appear. |
| Both filters | Search for `robot` and choose Technology | Only Robotics Challenge appears. |
| Clear | Click Clear | The controls reset and all cards appear. |

---

# Debugging Checklist

If something does not work, check one stage at a time:

1. **Select** — Is an element `null`?
2. **Iterate** — Does `cards.length` match the number of cards?
3. **Listen** — Does the event listener run?
4. **Read** — Are the input values correct?
5. **Decide** — Is `shouldShow` true or false as expected?
6. **Update** — Does the card receive `is-hidden`?
7. **Check** — Does the status message match the visible cards?

```js
console.log(query);
console.log(category);
console.log(shouldShow);
```

Use the browser Console to see these values while testing.
