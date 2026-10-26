# Activity Directory

A beginner-friendly project for learning how HTML, CSS, and JavaScript work together to create an interactive webpage.

---

## What You Will Learn

By the end of this activity, you should be able to:

- Explain the different jobs of HTML, CSS, and JavaScript.
- Find HTML elements using JavaScript.
- Respond to user actions such as clicks and form submissions.
- Read text from an input field and a dropdown menu.
- Show and hide activity cards.
- Update a message on the page.
- Debug a simple interactive webpage.

---

## What You Will Build

You will create an **Activity Directory**.

A user will be able to:

- Type a word such as `robot`.
- Choose a category such as `Technology`.
- Click **Search**.
- See only matching activity cards.
- Click **Clear** to show every activity again.

---

# Before the Practical

Do not start copying code yet.

First, understand the three files used to build a webpage.

---

## HTML, CSS, and JavaScript

A webpage uses three languages that work together.

![Diagram showing how HTML creates the page, JavaScript decides what happens, and CSS displays the result.](pics/html-css-js.png)

| Language | Simple job | Example in this project |
|---|---|---|
| HTML | Creates page content | Search box, buttons, and activity cards |
| CSS | Controls appearance | Card layout, spacing, colours, and hidden cards |
| JavaScript | Adds behaviour | Reads the search word and filters the cards |

A simple way to remember this is:

> **HTML creates. CSS styles. JavaScript responds.**

---

## HTML: Creates the Content

HTML creates the parts of the webpage that a user can see or use.

In this project, HTML creates:

- The page heading.
- The search input.
- The category menu.
- The Search button.
- The Clear button.
- The status message.
- The activity cards.

Example:

```html
<input id="keyword" type="text">
```

This creates a text input box.

The `id` gives the input box a special name:

```text
keyword
```

JavaScript can use this name later to find the input.

Example:

```html
<article class="activity-card sports">
  <h2>Basketball Club</h2>
  <p>Play weekly basketball games with other students.</p>
</article>
```

This creates one activity card.

| Part | Meaning |
|---|---|
| `activity-card` | This element is an activity card |
| `sports` | This card belongs to the Sports category |

> **Remember:** HTML creates the page and gives JavaScript elements to find.

---

## CSS: Controls Appearance

CSS controls how HTML looks on the webpage.

In this project, CSS controls:

- The page font and colours.
- Space between items.
- The activity card layout.
- The appearance of buttons.
- Whether an activity card is visible or hidden.

Example:

```css
.activity-card {
  padding: 16px;
  background: white;
  border-left: 5px solid #2563eb;
}
```

This makes each activity look like a card.

The most important CSS rule is:

```css
.is-hidden {
  display: none;
}
```

This means:

> If an element has the `is-hidden` class, do not display it.

Visible card:

```html
<article class="activity-card sports">
```

Hidden card:

```html
<article class="activity-card sports is-hidden">
```

> **Remember:** CSS controls what the user sees.

---

## JavaScript: Adds Behaviour

JavaScript makes the webpage respond when a user does something.

In this project, JavaScript:

1. Finds the HTML elements.
2. Waits for the user to interact with the page.
3. Reads the search word and selected category.
4. Checks each activity card.
5. Decides whether each card matches.
6. Hides cards that do not match.
7. Updates the number of visible cards.

Example:

```js
const keywordInput = document.querySelector("#keyword");
```

This means:

> Find the HTML element with the id `keyword`.

Example:

```js
clearButton.addEventListener("click", function () {
  keywordInput.value = "";
});
```

This means:

> When the user clicks Clear, empty the search input.

Example:

```js
card.classList.toggle("is-hidden", !shouldShow);
```

This means:

> Hide the card if it should not be shown. Otherwise, show it.

> **Remember:** JavaScript listens, reads, decides, and updates.

---

## How the Three Files Work Together

Imagine that the user:

1. Types `robot`.
2. Chooses `Technology`.
3. Clicks Search.

This is what happens:

| Step | What happens |
|---|---|
| HTML | Provides the input, menu, Search button, and activity cards |
| JavaScript | Reads `robot` and `technology` |
| JavaScript | Checks every activity card |
| JavaScript | Hides cards that do not match |
| CSS | Hides cards with the `is-hidden` class |
| HTML | Shows the updated status message on the page |

---

## How the App Works

When a user searches or changes a category, JavaScript follows the same sequence.

![Diagram showing the DOM interaction steps: Select, Iterate, Listen, Read, Decide, Update, and Check.](pics/dom-steps.png)

1. **Select** — Find the HTML elements.
2. **Iterate** — Check one activity card at a time.
3. **Listen** — Wait for a user action.
4. **Read** — Read the current values.
5. **Decide** — Work out whether a card matches.
6. **Update** — Show or hide cards and update text.
7. **Check** — Confirm that the result is correct.

If something does not work, check these steps in the same order. The first step that fails is usually where the problem is.

---

## Find HTML Elements

JavaScript needs to find an HTML element before it can use it.

![Diagram comparing `querySelector()` for one element and `querySelectorAll()` for many activity cards.](pics/selectors.png)

Use `querySelector()` to find **one** element.

```js
const keywordInput = document.querySelector("#keyword");
```

Use `querySelectorAll()` to find **many** elements.

```js
const cards = document.querySelectorAll(".activity-card");
```

| HTML name | JavaScript selector | Meaning |
|---|---|---|
| `id="keyword"` | `"#keyword"` | Find one unique element |
| `class="activity-card"` | `".activity-card"` | Find all matching cards |

`querySelectorAll()` returns a collection of cards called a **NodeList**.

Use `forEach()` to check one card at a time.

```js
cards.forEach(function (card) {
  console.log(card.textContent);
});
```

---

## Events and Values

An **event** is something the user does on the webpage.

![Diagram showing events for typing, choosing a category, submitting Search, and clicking Clear.](pics/events-values.png)

| User action | Event | Used for |
|---|---|---|
| User types in the search box | `input` | Optional live search |
| User chooses a category | `change` | Filter after the dropdown changes |
| User clicks Search or presses Enter | `submit` | Search the activity directory |
| User clicks Clear | `click` | Reset the filters |

Read the current values like this:

```js
const query = keywordInput.value.trim();
```

```js
const category = categorySelect.value;
```

- `.value` reads the value from an input or dropdown.
- `.trim()` removes spaces before and after the search word.

---

## Check Your Understanding

Before starting the practical, explain the following in your own words:

1. What does HTML create?
2. What does CSS control?
3. What does JavaScript do after a user clicks Search?
4. Why does JavaScript need to find an HTML element first?
5. What does the `is-hidden` class do?

When you can answer these questions, you are ready to start.

---

# Practical: Build the Activity Directory

## Project Structure

Create a folder called `activity-directory`.

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

---

## Step 1: Create `index.html`

Create a file named `index.html`.

Copy this code into it:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Activity Directory</title>

  <link rel="stylesheet" href="style.css">
  <script src="app.js" defer></script>
</head>

<body>
  <main class="container">
    <h1>Activity Directory</h1>

    <form id="search-form">
      <label for="keyword">Search activities</label>

      <input
        id="keyword"
        type="text"
        placeholder="Try: coding"
      >

      <label for="category">Choose a category</label>

      <select id="category">
        <option value="all">All categories</option>
        <option value="sports">Sports</option>
        <option value="technology">Technology</option>
        <option value="arts">Arts</option>
      </select>

      <button type="submit">Search</button>
      <button type="button" id="clear-button">Clear</button>
    </form>

    <p id="status">6 activities found</p>

    <section class="activity-list">
      <article class="activity-card sports">
        <h2>Basketball Club</h2>
        <p>Play weekly basketball games with other students.</p>
      </article>

      <article class="activity-card technology">
        <h2>Coding Workshop</h2>
        <p>Learn JavaScript and create simple web applications.</p>
      </article>

      <article class="activity-card arts">
        <h2>Digital Drawing</h2>
        <p>Create illustrations using digital design tools.</p>
      </article>

      <article class="activity-card sports">
        <h2>Badminton Training</h2>
        <p>Improve your badminton skills and fitness.</p>
      </article>

      <article class="activity-card technology">
        <h2>Robotics Challenge</h2>
        <p>Build and program a small robot with your team.</p>
      </article>

      <article class="activity-card arts">
        <h2>Photography Walk</h2>
        <p>Practise taking photos around the campus.</p>
      </article>
    </section>
  </main>
</body>
</html>
```

### Why use `defer`?

```html
<script src="app.js" defer></script>
```

`defer` tells the browser to read the HTML first and run JavaScript afterwards.

Without `defer`, JavaScript may run too early and fail to find the HTML elements.

---

## Step 2: Create `style.css`

Create a file named `style.css`.

Copy this code into it:

```css
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f4f6f8;
  color: #1f2933;
}

.container {
  width: 90%;
  max-width: 900px;
  margin: 40px auto;
}

form {
  display: grid;
  gap: 10px;
  padding: 20px;
  background: white;
}

input,
select,
button {
  padding: 10px;
  font-size: 16px;
}

.activity-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 16px;
  margin-top: 20px;
}

.activity-card {
  padding: 16px;
  background: white;
  border-left: 5px solid #2563eb;
}

.is-hidden {
  display: none;
}
```

Save the file and refresh the browser.

You should now see a styled page with six activity cards.

---

## Step 3: Start `app.js`

Create a file named `app.js`.

Copy this code into it:

```js
const form = document.querySelector("#search-form");
const keywordInput = document.querySelector("#keyword");
const categorySelect = document.querySelector("#category");
const clearButton = document.querySelector("#clear-button");
const status = document.querySelector("#status");

const cards = document.querySelectorAll(".activity-card");
```

### Check Your Selectors

Temporarily add this below the code:

```js
console.log(form);
console.log(keywordInput);
console.log(categorySelect);
console.log(clearButton);
console.log(status);
console.log(cards.length);
```

Open the webpage in a browser.

Press `F12`, then choose the **Console** tab.

You should see:

- HTML elements instead of `null`.
- The number `6` for `cards.length`.

Remove the `console.log()` lines after checking.

---

## Step 4: Add the Filter Function

Add this code below your selectors:

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

### Understand the Filter Function

```js
const matchesKeyword = cardText.includes(query.toLowerCase());
```

This checks whether the activity text contains the search word.

```js
const matchesCategory =
  category === "all" || card.classList.contains(category);
```

This checks whether the category is All categories or whether the card matches the chosen category.

```js
const shouldShow = matchesKeyword && matchesCategory;
```

The `&&` means **AND**.

The card should appear only when:

- The keyword matches.
- The category matches.

```js
card.classList.toggle("is-hidden", !shouldShow);
```

This adds `is-hidden` when the card should not appear.

```js
status.textContent = visibleCount + " activities found";
```

This updates the message on the page.

---

## Step 5: Connect the Search Form

Add this below the filter function:

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

## Step 6: Connect the Category Menu

Add this code below the form listener:

```js
categorySelect.addEventListener("change", function () {
  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

When the user chooses another category, JavaScript runs the filter again.

---

## Step 7: Connect the Clear Button

Add this code below the category listener:

```js
clearButton.addEventListener("click", function () {
  keywordInput.value = "";
  categorySelect.value = "all";

  filterActivities("", "all");
});
```

This clears the search word, resets the category, and shows all cards again.

---

## Step 8: Test Your Work

Test each case one at a time.

| Test | What to do | Expected result |
|---|---|---|
| Show all | Keep the input empty and choose All categories | All six cards appear |
| Keyword search | Search for `coding` | Only Coding Workshop appears |
| No match | Search for `swimming` | No cards appear and the message shows `0 activities found` |
| Category filter | Choose Sports | Only Basketball Club and Badminton Training appear |
| Both filters | Search for `robot` and choose Technology | Only Robotics Challenge appears |
| Clear | Click Clear | The input and category reset; all six cards appear |

---

# Debugging Checklist

If the webpage does not work, check one step at a time.

1. **Select** — Is a selected element `null`?
2. **Iterate** — Does `cards.length` match the number of activity cards?
3. **Listen** — Does the event listener run?
4. **Read** — Are `query` and `category` correct?
5. **Decide** — Is `shouldShow` true or false when expected?
6. **Update** — Does the hidden card have the `is-hidden` class?
7. **Check** — Does the status count match the cards on screen?

Use these temporary checks:

```js
console.log(query);
console.log(category);
console.log(shouldShow);
```

---

# Optional Challenges

Complete these only after the main activity works.

## Live Search

Filter cards while the user types.

```js
keywordInput.addEventListener("input", function () {
  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

## Clear with Escape

Clear the filter when the user presses Escape.

```js
document.addEventListener("keydown", function (event) {
  if (event.key === "Escape") {
    keywordInput.value = "";
    categorySelect.value = "all";

    filterActivities("", "all");
  }
});
```

## Add an Activity Card

Try adding another activity card by using:

- `document.createElement()`
- `textContent`
- `classList.add()`
- `append()`
