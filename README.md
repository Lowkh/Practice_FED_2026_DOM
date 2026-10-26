# Activity Directory

A simple beginner project to learn how JavaScript can make a webpage respond to users.

---

## What You Will Build

You will create a webpage with:

- A box where the user can type a search word.
- A dropdown menu for choosing a category.
- A **Search** button.
- A **Clear** button.
- Activity cards that can appear or disappear.
- A message showing how many activities are visible.

For example:

- Search for `robot`
- Choose `Technology`
- Only **Robotics Challenge** should appear

---

## The Big Idea

A webpage uses three languages:

| Language | What it does |
|---|---|
| HTML | Creates the content on the page |
| CSS | Makes the page look better |
| JavaScript | Makes the page respond when the user does something |

For this project:

1. HTML creates the input, dropdown, buttons, message, and cards.
2. JavaScript reads what the user chooses.
3. JavaScript decides which cards should be shown.
4. CSS hides cards that should not be shown.

The browser creates a live version of the HTML called the **DOM**. JavaScript uses the DOM to find and change things on the page. [file:1]

---

## Create Your Files

Create a folder called:

```text
activity-directory
```

Inside the folder, create these files:

```text
activity-directory/
├── index.html
├── style.css
├── app.js
└── README.md
```

| File | Job |
|---|---|
| `index.html` | Creates the webpage |
| `style.css` | Styles the webpage |
| `app.js` | Adds JavaScript behaviour |
| `README.md` | Explains the project |

---

# Step 1: Create the HTML

Open `index.html`.

Copy and paste this code:

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

---

## Understand the HTML

### What is an `id`?

An `id` is a special name for one HTML element.

```html
<input id="keyword">
```

The input box has the name `keyword`.

JavaScript can find it using:

```js
document.querySelector("#keyword");
```

The `#` means “find this `id`.”

---

### What is a `class`?

A class is a label that can be used by many elements.

```html
<article class="activity-card sports">
```

This activity has two labels:

- `activity-card` means it is an activity card.
- `sports` means it belongs to the sports category.

---

### Why is `defer` important?

This line connects JavaScript to HTML:

```html
<script src="app.js" defer></script>
```

`defer` tells the browser:

> Read all the HTML first. Run JavaScript after that.

Without `defer`, JavaScript may try to find an element before the element exists. The result may be `null`, which means “JavaScript did not find anything.” [file:1]

---

# Step 2: Add the CSS

Open `style.css`.

Copy and paste this code:

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

---

## Important CSS Rule

This is the most important rule:

```css
.is-hidden {
  display: none;
}
```

`display: none` means:

> Do not show this element on the webpage.

Later, JavaScript will add `is-hidden` to cards that do not match the search.

Example:

```html
<article class="activity-card sports is-hidden">
```

This card will disappear.

---

# Step 3: Let JavaScript Find the HTML

Open `app.js`.

Copy and paste this code:

```js
const form = document.querySelector("#search-form");
const keywordInput = document.querySelector("#keyword");
const categorySelect = document.querySelector("#category");
const clearButton = document.querySelector("#clear-button");
const status = document.querySelector("#status");

const cards = document.querySelectorAll(".activity-card");
```

---

## What does this code do?

JavaScript needs to find the HTML elements before it can use them.

```js
const keywordInput = document.querySelector("#keyword");
```

This means:

> Find the HTML element with the id `keyword` and save it in `keywordInput`.

`const` creates a variable that stores something.

Think of a variable as a labelled box:

```text
keywordInput = the search input box
```

---

## One element or many elements?

Use `querySelector()` when you expect one item.

```js
const status = document.querySelector("#status");
```

Use `querySelectorAll()` when you expect many items.

```js
const cards = document.querySelectorAll(".activity-card");
```

`cards` contains all six activity cards.

---

## Check That JavaScript Found Everything

Add this temporary code below your selection code:

```js
console.log(form);
console.log(keywordInput);
console.log(categorySelect);
console.log(clearButton);
console.log(status);
console.log(cards.length);
```

Open `index.html` in a browser.

Press `F12`, then choose the **Console** tab.

You should see:

- HTML elements for the first five lines.
- The number `6` on the last line.

If you see `null`, check that the HTML `id` and JavaScript selector are exactly the same.

Remove the `console.log()` lines after you have checked them.

---

# Step 4: Check Every Activity Card

JavaScript must check the activity cards one at a time.

Add this temporary code:

```js
cards.forEach(function (card) {
  console.log(card.textContent);
});
```

Refresh the browser and look at the Console.

You should see the text from every activity card.

---

## What does `forEach()` mean?

`forEach()` means:

> Do this once for every item.

In this code:

```js
cards.forEach(function (card) {
```

- `cards` means all the activity cards.
- `forEach()` goes through them one by one.
- `card` means one current card.

After testing, remove the temporary code.

---

# Step 5: Create the Filter Function

Add this code after the selection code:

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

Do not worry if the whole function looks difficult at first. Read it one small part at a time.

---

## The Function Name

```js
function filterActivities(query, category) {
```

This creates a function named `filterActivities`.

A function is a set of instructions that JavaScript can run later.

This function needs two pieces of information:

- `query` is the word typed by the user.
- `category` is the option chosen by the user.

Example:

```js
filterActivities("robot", "technology");
```

This means:

> Search for `robot` in Technology activities.

---

## Count Visible Cards

```js
let visibleCount = 0;
```

This starts a counter at zero.

Every time JavaScript finds a matching card, it adds one.

---

## Read the Card Text

```js
const cardText = card.textContent.toLowerCase();
```

This reads all text inside one card.

`toLowerCase()` changes capital letters to lowercase letters.

For example:

```text
Robotics Challenge
```

becomes:

```text
robotics challenge
```

This helps users search using either `robot`, `ROBOT`, or `Robot`.

---

## Check the Search Word

```js
const matchesKeyword = cardText.includes(query.toLowerCase());
```

`includes()` asks:

> Does this card contain the search word?

Example:

```js
"robotics challenge".includes("robot");
```

The answer is:

```js
true
```

Example:

```js
"robotics challenge".includes("coding");
```

The answer is:

```js
false
```

---

## Check the Category

```js
const matchesCategory =
  category === "all" || card.classList.contains(category);
```

This checks whether the card is in the selected category.

The `||` symbol means **OR**.

The statement means:

> Show the card if the user selected All categories OR if the card has the selected category class.

For example:

```js
card.classList.contains("sports");
```

checks whether the current card has the class `sports`.

---

## Make the Final Decision

```js
const shouldShow = matchesKeyword && matchesCategory;
```

The `&&` symbol means **AND**.

A card should show only when:

- The keyword matches **AND**
- The category matches.

| Keyword matches | Category matches | Show card? |
|---|---|---|
| Yes | Yes | Yes |
| Yes | No | No |
| No | Yes | No |
| No | No | No |

---

## Hide or Show the Card

```js
card.classList.toggle("is-hidden", !shouldShow);
```

This line decides whether to add or remove the `is-hidden` class.

- If the card should show, remove `is-hidden`.
- If the card should not show, add `is-hidden`.

Remember this CSS:

```css
.is-hidden {
  display: none;
}
```

That is why the card disappears.

---

## Update the Number on the Page

```js
status.textContent = visibleCount + " activities found";
```

This changes the status message.

For example, if two cards match:

```text
2 activities found
```

---

# Step 6: Make the Search Button Work

Add this code after the function:

```js
form.addEventListener("submit", function (event) {
  event.preventDefault();

  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

---

## What is an Event?

An event is something a user does.

Examples:

- Click a button.
- Type in a box.
- Choose something from a dropdown menu.
- Submit a form.

This code listens for a form submission:

```js
form.addEventListener("submit", function (event) {
```

When the user clicks Search or presses Enter, the code inside runs.

---

## Why Use `preventDefault()`?

Normally, a form reloads the webpage after it is submitted.

```js
event.preventDefault();
```

stops the page from reloading.

This lets JavaScript update the cards on the same page.

---

## Read User Choices

```js
const query = keywordInput.value.trim();
```

This reads what the user typed.

```js
const category = categorySelect.value;
```

This reads what the user selected from the dropdown menu.

Then this line runs the filter:

```js
filterActivities(query, category);
```

---

# Step 7: Make the Dropdown Work

Add this code after the Search event code:

```js
categorySelect.addEventListener("change", function () {
  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

The `change` event happens when the user chooses a different category.

---

# Step 8: Make the Clear Button Work

Add this code after the dropdown event code:

```js
clearButton.addEventListener("click", function () {
  keywordInput.value = "";
  categorySelect.value = "all";

  filterActivities("", "all");
});
```

The `click` event happens when the user clicks Clear.

This code:

1. Removes the search word.
2. Changes the dropdown back to All categories.
3. Shows every card again.

---

# Completed `app.js`

Your final `app.js` should be:

```js
const form = document.querySelector("#search-form");
const keywordInput = document.querySelector("#keyword");
const categorySelect = document.querySelector("#category");
const clearButton = document.querySelector("#clear-button");
const status = document.querySelector("#status");

const cards = document.querySelectorAll(".activity-card");

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

form.addEventListener("submit", function (event) {
  event.preventDefault();

  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});

categorySelect.addEventListener("change", function () {
  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});

clearButton.addEventListener("click", function () {
  keywordInput.value = "";
  categorySelect.value = "all";

  filterActivities("", "all");
});
```

---

# Test Your Work

Try each test one at a time.

| Test | What to do | What should happen |
|---|---|---|
| Show all | Leave the search box empty. Choose All categories. | Six cards should show. |
| Search | Type `coding`. Click Search. | Only Coding Workshop should show. |
| No result | Type `swimming`. Click Search. | No cards should show. The message should say `0 activities found`. |
| Category | Choose Sports. | Basketball Club and Badminton Training should show. |
| Both filters | Type `robot`. Choose Technology. Click Search. | Only Robotics Challenge should show. |
| Clear | Click Clear. | All six cards should show again. |

---

# If Something Does Not Work

Check one thing at a time.

## 1. Check the selectors

Add:

```js
console.log(keywordInput);
console.log(cards.length);
```

- If you see `null`, check the `id` in HTML.
- If you see `0`, check the `activity-card` class name.

## 2. Check whether the button is clicked

Add:

```js
console.log("Button was clicked");
```

inside the Clear button event.

## 3. Check what JavaScript reads

Add:

```js
console.log(query);
console.log(category);
```

inside an event listener.

## 4. Check whether a card has `is-hidden`

Right-click a hidden card, choose **Inspect**, and look for:

```html
is-hidden
```

The lesson’s recommended approach is to debug in this order: select, listen, read, decide, update, then check the visible result. [file:1]

---

# Optional Challenge

After the main project works, make the search update while the user types:

```js
keywordInput.addEventListener("input", function () {
  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

The `input` event happens whenever the user edits the text field. The lesson also introduces `keydown` for a deliberate key command, such as using Escape to clear a filter. [file:1]
