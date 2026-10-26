# Activity Directory Practical

## DOM Interaction and Events for Beginners

In this practical, you will build an **Activity Directory** webpage.

The webpage will allow users to:

- Search for activities using a keyword.
- Filter activities by category.
- Clear the search and show all activities again.
- See how many activities are currently visible.

This practical teaches the relationship between:

- **HTML** — creates the page structure and content.
- **CSS** — controls the appearance of the page.
- **JavaScript** — responds to user actions and changes the page.

---

## Learning Outcomes

By the end of this practical, you should be able to:

- Select HTML elements with JavaScript.
- Explain what the DOM is.
- Use `querySelector()` and `querySelectorAll()`.
- Use `forEach()` to work with multiple elements.
- Listen for `submit`, `change`, and `click` events.
- Read values from an input field and dropdown menu.
- Use conditions to decide whether an activity should be shown.
- Add and remove a CSS class with `classList.toggle()`.
- Update visible text with `textContent`.
- Debug an interaction step by step.

---

## Before You Begin

Create a folder named:

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

Each file has a different role:

| File | Purpose |
|---|---|
| `index.html` | Creates the content and structure of the webpage |
| `style.css` | Controls the appearance of the webpage |
| `app.js` | Adds interaction and behaviour |
| `README.md` | Explains the practical and how the code works |

---

# Part 1: The Main Interaction Pattern

Most interactive webpages follow this pattern:

1. **Select** — Find HTML elements using JavaScript.
2. **Listen** — Wait for a user action.
3. **Read** — Get the user’s current input.
4. **Decide** — Work out what should happen.
5. **Update** — Change text, classes, or elements on the page.
6. **Check** — Confirm that the result is correct.

For this practical:

| Step | Example |
|---|---|
| Select | Find the search input and activity cards |
| Listen | Wait for the user to submit the form |
| Read | Read the keyword and chosen category |
| Decide | Check whether each card matches both filters |
| Update | Hide cards that do not match |
| Check | Confirm that the status count matches visible cards |

---

# Part 2: Create the HTML

## Step 1: Add `index.html`

Copy the following code into `index.html`.

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
        placeholder="For example: coding"
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

    <p id="status" class="status">6 activities found</p>

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

## Understanding the HTML

### What is an `id`?

An `id` is a unique name for one element.

```html
<input id="keyword">
```

JavaScript can find this element with:

```js
document.querySelector("#keyword");
```

The `#` means “select an element by its `id`.”

Each `id` should be used only once on a page.

---

### What is a `class`?

A class is a label that can be shared by more than one element.

```html
<article class="activity-card sports">
```

This activity card has two classes:

- `activity-card` identifies it as an activity card.
- `sports` identifies its category.

JavaScript can find every activity card with:

```js
document.querySelectorAll(".activity-card");
```

The `.` means “select an element by its class.”

---

### What is `defer`?

This line loads JavaScript:

```html
<script src="app.js" defer></script>
```

The `defer` attribute tells the browser:

> Load `app.js`, but wait until the HTML has been read before running it.

Without `defer`, JavaScript may run too early. It may try to find an HTML element before the browser has created it.

When JavaScript cannot find an element, it may return:

```js
null
```

`null` means that no matching element was found.

---

# Part 3: Add the CSS

## Step 2: Add `style.css`

Copy the following code into `style.css`.

```css
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f4f6f8;
  color: #1f2933;
}

.container {
  width: min(900px, 90%);
  margin: 40px auto;
}

form {
  display: grid;
  gap: 10px;
  padding: 20px;
  background: white;
  border-radius: 10px;
}

input,
select,
button {
  padding: 10px;
  font-size: 1rem;
}

button {
  cursor: pointer;
}

.status {
  margin: 20px 0;
  font-weight: bold;
  color: #17785b;
}

.activity-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 16px;
}

.activity-card {
  padding: 18px;
  background: white;
  border-left: 5px solid #2563eb;
  border-radius: 8px;
}

.activity-card h2 {
  margin-top: 0;
}

.is-hidden {
  display: none;
}
```

---

## Understanding CSS states

The most important CSS rule in this practical is:

```css
.is-hidden {
  display: none;
}
```

`display: none` hides an element from the page.

At first, an activity card looks like this:

```html
<article class="activity-card sports">
```

When JavaScript hides it, the card becomes:

```html
<article class="activity-card sports is-hidden">
```

Because the card has the `is-hidden` class, CSS does not display it.

The three technologies have different jobs:

| Technology | Responsibility |
|---|---|
| HTML | Creates the activity card |
| JavaScript | Adds or removes `is-hidden` |
| CSS | Hides an element with `is-hidden` |

---

# Part 4: Select HTML Elements

## Step 3: Start `app.js`

Copy the following code into `app.js`.

```js
const form = document.querySelector("#search-form");
const keywordInput = document.querySelector("#keyword");
const categorySelect = document.querySelector("#category");
const clearButton = document.querySelector("#clear-button");
const status = document.querySelector("#status");

const cards = document.querySelectorAll(".activity-card");
```

---

## What is the DOM?

DOM means **Document Object Model**.

The browser converts your HTML page into a live structure that JavaScript can read and change.

JavaScript can use the DOM to:

- Read text.
- Read user input.
- Change visible text.
- Add or remove CSS classes.
- Create new elements.
- Remove elements.

For example, HTML creates the input field:

```html
<input id="keyword">
```

JavaScript selects it:

```js
const keywordInput = document.querySelector("#keyword");
```

The variable `keywordInput` now refers to the actual text input on the webpage.

---

## `querySelector()` versus `querySelectorAll()`

Use `querySelector()` when you expect to find one element.

```js
const status = document.querySelector("#status");
```

It returns:

- One element if found.
- `null` if no element matches.

Use `querySelectorAll()` when you expect to find multiple elements.

```js
const cards = document.querySelectorAll(".activity-card");
```

It returns a **NodeList**, which is a collection of matching elements.

---

## Check your selections

Temporarily add this below your selection code.

```js
console.log(form);
console.log(keywordInput);
console.log(categorySelect);
console.log(clearButton);
console.log(status);
console.log(cards.length);
```

Open `index.html` in a browser.

Then open the browser Console:

1. Press `F12`, or right-click the webpage.
2. Select **Inspect**.
3. Select the **Console** tab.

Expected result:

- The first five lines should display HTML elements.
- None of them should be `null`.
- The final line should show `6`.

```text
6
```

If you see `null`, check whether the HTML `id` and JavaScript selector match exactly.

For example, these must match:

```html
<form id="search-form">
```

```js
document.querySelector("#search-form");
```

Remove the temporary `console.log()` statements once you have confirmed that your selections work.

---

# Part 5: Work with Multiple Cards

## Step 4: Learn `forEach()`

There are six activity cards. JavaScript must check each card individually.

Try this temporary code:

```js
cards.forEach(function (card) {
  console.log(card.textContent);
});
```

Refresh the page and look in the Console.

You should see text from all six activity cards.

---

## What does `forEach()` do?

`forEach()` means:

> Repeat the same instructions for every item in a collection.

In this example:

```js
cards.forEach(function (card) {
```

- `cards` is the collection of all activity cards.
- `forEach()` goes through every card.
- `card` represents one current activity card.

For example, when JavaScript reaches the Coding Workshop card, this line:

```js
card.textContent
```

reads its text.

Remove the temporary code after you understand how it works.

---

# Part 6: Filter the Activities

## Step 5: Add the filtering function

Add this code below the element selection code.

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

This function checks every activity card and decides whether it should be shown or hidden.

---

## Function parameters

This function receives two values:

```js
function filterActivities(query, category) {
```

| Parameter | Meaning |
|---|---|
| `query` | The text entered by the user |
| `category` | The category selected by the user |

For example:

```js
filterActivities("robot", "technology");
```

This means:

- Search for the word `robot`.
- Show only activities in the `technology` category.

---

## Start the visible count

```js
let visibleCount = 0;
```

This variable counts how many cards remain visible.

It starts at `0` because JavaScript has not checked any cards yet.

The keyword `let` is used because the value changes as JavaScript finds matching activity cards.

---

## Read the text from each card

```js
const cardText = card.textContent.toLowerCase();
```

`textContent` reads text inside an HTML element.

For the Robotics Challenge card, it might return:

```text
Robotics Challenge Build and program a small robot with your team.
```

`toLowerCase()` changes the text to lowercase:

```text
robotics challenge build and program a small robot with your team.
```

This allows the user to search without worrying about capital letters.

For example:

```js
"Robotics Challenge".toLowerCase()
```

becomes:

```text
robotics challenge
```

---

## Check whether the keyword matches

```js
const matchesKeyword = cardText.includes(query.toLowerCase());
```

`includes()` asks whether one piece of text contains another.

```js
"robotics challenge".includes("robot")
```

returns:

```js
true
```

```js
"robotics challenge".includes("coding")
```

returns:

```js
false
```

The result is stored in `matchesKeyword`.

It will always be either:

```js
true
```

or:

```js
false
```

---

## Check whether the category matches

```js
const matchesCategory =
  category === "all" || card.classList.contains(category);
```

This checks the category of the current activity card.

The operator `===` means “is exactly equal to.”

```js
category === "all"
```

is true when the user has selected **All categories**.

The operator `||` means **OR**.

The whole statement means:

> The category matches if the selected category is `all`, OR the card has the selected category class.

For this card:

```html
<article class="activity-card sports">
```

this code returns `true`:

```js
card.classList.contains("sports");
```

But this code returns `false`:

```js
card.classList.contains("technology");
```

---

## Decide whether to show the card

```js
const shouldShow = matchesKeyword && matchesCategory;
```

The operator `&&` means **AND**.

The card should show only when both conditions are true:

- The keyword matches.
- The category matches.

| Keyword matches | Category matches | Show the card? |
|---|---|---|
| `true` | `true` | Yes |
| `true` | `false` | No |
| `false` | `true` | No |
| `false` | `false` | No |

For example, searching for `robot` and choosing Technology shows Robotics Challenge because both conditions are true.

---

## Hide or show the card

```js
card.classList.toggle("is-hidden", !shouldShow);
```

This line adds or removes the `is-hidden` class.

The `!` symbol means **NOT**.

| `shouldShow` | `!shouldShow` | Result |
|---|---|---|
| `true` | `false` | Remove `is-hidden`; show the card |
| `false` | `true` | Add `is-hidden`; hide the card |

The CSS rule then hides cards with the class:

```css
.is-hidden {
  display: none;
}
```

---

## Increase the count

```js
if (shouldShow) {
  visibleCount = visibleCount + 1;
}
```

An `if` statement runs code only when its condition is true.

When the card should be visible, JavaScript increases the visible count by one.

If three cards match the filters, the final value becomes:

```js
visibleCount = 3;
```

---

## Update the status message

```js
status.textContent = visibleCount + " activities found";
```

This replaces the text inside this element:

```html
<p id="status" class="status">6 activities found</p>
```

If two activities are visible, the page shows:

```text
2 activities found
```

Use `textContent` for plain text messages.

---

# Part 7: Respond to User Actions

## Step 6: Listen for Search

Add this code below `filterActivities()`.

```js
form.addEventListener("submit", function (event) {
  event.preventDefault();

  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

---

## What is an event?

An event is something that happens in the browser.

Examples include:

- Clicking a button.
- Typing in a text field.
- Selecting a new dropdown option.
- Submitting a form.
- Pressing a key.

This line means:

```js
form.addEventListener("submit", function (event) {
```

> Wait for the user to submit the form. Then run the function.

---

## Why use `preventDefault()`?

A form normally reloads the page when it is submitted.

```js
event.preventDefault();
```

stops the normal reload behaviour.

This allows JavaScript to filter activity cards without leaving or refreshing the page.

---

## Read the user input

```js
const query = keywordInput.value.trim();
```

- `.value` reads what the user typed.
- `.trim()` removes spaces at the beginning and end.

For example:

```text
   coding   
```

becomes:

```text
coding
```

Read the selected dropdown value using:

```js
const category = categorySelect.value;
```

If the user chooses Technology, JavaScript reads:

```text
technology
```

Finally, send both values to the shared filter function:

```js
filterActivities(query, category);
```

---

## Step 7: Listen for category changes

Add this code below the form event listener.

```js
categorySelect.addEventListener("change", function () {
  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

The `change` event happens when the user chooses a different option from the dropdown menu.

This listener:

1. Reads the current keyword.
2. Reads the newly selected category.
3. Runs the same `filterActivities()` function.

The main filtering rules are written only once inside the shared function.

---

## Step 8: Listen for Clear button clicks

Add this code below the category event listener.

```js
clearButton.addEventListener("click", function () {
  keywordInput.value = "";
  categorySelect.value = "all";

  filterActivities("", "all");
});
```

The `click` event runs when the user clicks the Clear button.

This code:

1. Sets the text input to an empty string.
2. Changes the dropdown back to `all`.
3. Runs the filter again.
4. Shows every activity card.

---

# Part 8: Completed `app.js`

Your final `app.js` should look like this:

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

# Part 9: Test Your Work

Test one case at a time.

| Test | Action | Expected result |
|---|---|---|
| Show all activities | Leave the text field empty and select All categories | All six activity cards appear |
| Keyword search | Type `coding`, then click Search | Only Coding Workshop appears |
| No result | Type `swimming`, then click Search | No cards appear and the status says `0 activities found` |
| Category filter | Select Sports | Basketball Club and Badminton Training appear |
| Combined filter | Type `robot`, select Technology, then click Search | Only Robotics Challenge appears |
| Reset filters | Click Clear | All six cards appear again |

---

# Part 10: Debugging Checklist

If your work does not behave as expected, debug in this order.

## 1. Select

Check whether JavaScript found the HTML elements.

```js
console.log(keywordInput);
console.log(categorySelect);
console.log(cards.length);
```

- If an element is `null`, check the HTML `id` and JavaScript selector.
- If `cards.length` is `0`, check that every card has `class="activity-card"`.

## 2. Listen

Check whether an event happens.

```js
clearButton.addEventListener("click", function () {
  console.log("Clear button clicked");
});
```

Click the Clear button and check the Console.

## 3. Read

Check the values JavaScript reads.

```js
console.log(query);
console.log(category);
```

Make sure they match the user’s input and selected option.

## 4. Decide

Check the filtering conditions.

```js
console.log(matchesKeyword);
console.log(matchesCategory);
console.log(shouldShow);
```

## 5. Update

Inspect a hidden activity card in the browser.

It should contain the class:

```html
is-hidden
```

## 6. Check

Count the visible activity cards and compare the result with the status message.

> Start at the first step that does not work. Fix that step before changing later code.

---

# Optional Challenges

Complete these only after all required tests work.

## Challenge 1: Live filtering

Filter activities as the user types.

```js
keywordInput.addEventListener("input", function () {
  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

The `input` event runs whenever the text changes.

---

## Challenge 2: Press Escape to clear filters

Add this code:

```js
document.addEventListener("keydown", function (event) {
  if (event.key === "Escape") {
    keywordInput.value = "";
    categorySelect.value = "all";

    filterActivities("", "all");
  }
});
```

The `keydown` event runs when a key is pressed.

The condition checks whether the key is `Escape`.

---

## Challenge 3: Add another activity card

Add a new activity card manually in `index.html`.

Use one of the existing categories:

```html
<article class="activity-card technology">
  <h2>Web Design Club</h2>
  <p>Learn how to design and improve webpages.</p>
</article>
```

Then update the starting status message in the HTML from:

```html
<p id="status" class="status">6 activities found</p>
```

to:

```html
<p id="status" class="status">7 activities found</p>
```

---

# Reflection

After completing the practical, explain the following in your own words:

1. Why does JavaScript use `querySelectorAll(".activity-card")` instead of `querySelector(".activity-card")`?
2. Why does the code use `forEach()`?
3. What is the difference between `&&` and `||` in the filter function?
4. Why does adding the `is-hidden` class make a card disappear?
5. Why does the form need `event.preventDefault()`?
6. When should an interface use `input`, `change`, `submit`, or `click`?
