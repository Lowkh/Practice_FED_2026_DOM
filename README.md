# Activity Directory

A beginner-friendly front-end practical that demonstrates DOM selection, form events, filtering logic, and CSS state management using HTML, CSS, and JavaScript.

---

## Overview

This project implements an Activity Directory where users can:

- Search for activities using a keyword.
- Filter activities by category.
- Combine keyword and category filters.
- Reset all filters using a Clear button.
- View the number of matching activities.

The project demonstrates the relationship between:

| Technology | Responsibility |
|---|---|
| HTML | Provides the page structure, controls, and activity data. |
| CSS | Defines the visual presentation, including visible and hidden states. |
| JavaScript | Reads user input, applies filtering logic, and updates the page. |

---

## Learning Objectives

After completing this practical, you should be able to:

- Select single and multiple HTML elements using JavaScript.
- Explain the purpose of the Document Object Model (DOM).
- Use `querySelector()` and `querySelectorAll()` appropriately.
- Use `forEach()` to process a collection of elements.
- Attach event listeners using `submit`, `change`, and `click`.
- Read values from text inputs and dropdown menus.
- Use Boolean logic to determine whether an item should be shown.
- Update CSS classes using `classList.toggle()`.
- Update page text safely using `textContent`.
- Debug DOM interactions systematically.

---

## Project Structure

```text
activity-directory/
├── index.html
├── style.css
├── app.js
└── README.md
```

| File | Description |
|---|---|
| `index.html` | Defines the webpage content and structure. |
| `style.css` | Defines the visual layout and display states. |
| `app.js` | Adds filtering and event-driven behaviour. |
| `README.md` | Documents the project and explains the key concepts. |

---

## Application Workflow

The application follows this interaction sequence:

```text
Select → Listen → Read → Decide → Update → Check
```

| Stage | Application behaviour |
|---|---|
| Select | JavaScript locates the form, controls, status message, and cards. |
| Listen | JavaScript waits for form submission, category changes, or Clear button clicks. |
| Read | JavaScript reads the keyword and selected category. |
| Decide | JavaScript checks whether each card matches both filters. |
| Update | JavaScript hides non-matching cards and updates the result count. |
| Check | The visible cards and status message should agree. |

---

# Setup Instructions

## Prerequisites

You need:

- A modern web browser, such as Google Chrome, Microsoft Edge, Firefox, or Safari.
- A code editor, such as Visual Studio Code.
- Basic familiarity with HTML, CSS, and JavaScript files.

No external libraries or installations are required.

## Run the Project

1. Download or create the project files.
2. Open the `activity-directory` folder in a code editor.
3. Open `index.html` in a web browser.
4. Use the search field, category dropdown, and buttons to test the application.
5. Open Developer Tools with `F12` to inspect the Console when debugging.

---

# Implementation

## HTML: `index.html`

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

### HTML Notes

- An `id` identifies one unique element, such as `keyword` or `search-form`.
- A `class` groups elements with shared behaviour or styling, such as `activity-card`.
- Each activity has a category class, such as `sports`, `technology`, or `arts`.
- The `defer` attribute ensures that `app.js` runs only after HTML has been parsed.

```html
<script src="app.js" defer></script>
```

Without `defer`, JavaScript may run before the browser has created the target elements, causing selectors to return `null`.

---

## CSS: `style.css`

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

### CSS State Management

The `.is-hidden` class represents a visual state.

```css
.is-hidden {
  display: none;
}
```

JavaScript does not directly style every hidden card. Instead, it adds or removes the `is-hidden` class. CSS then determines that an element with this class should not be displayed.

---

## JavaScript: `app.js`

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

# Key Concepts

## DOM Selection

The DOM is the browser’s live representation of the HTML page.

Use `querySelector()` to find one element:

```js
const status = document.querySelector("#status");
```

Use `querySelectorAll()` to find multiple elements:

```js
const cards = document.querySelectorAll(".activity-card");
```

`querySelectorAll()` returns a NodeList. A NodeList may contain zero or more elements, and `forEach()` processes each element individually.

```js
cards.forEach(function (card) {
  console.log(card.textContent);
});
```

---

## Event Handling

An event is an action that occurs in the browser.

| Event | Used on | Purpose |
|---|---|---|
| `submit` | Form | Performs the search, including when the user presses Enter. |
| `change` | Category dropdown | Filters when the selected category changes. |
| `click` | Clear button | Resets the filters. |
| `input` | Text input | Optional event for live filtering while the user types. |
| `keydown` | Document | Optional event for keyboard shortcuts, such as Escape. |

The form submit listener uses `event.preventDefault()` to stop the browser from reloading the page.

```js
event.preventDefault();
```

---

## Reading Values

Different elements use different properties.

| Element | Property | Example result |
|---|---|---|
| Text input | `.value` | `"coding"` |
| Dropdown menu | `.value` | `"technology"` |
| Checkbox | `.checked` | `true` or `false` |
| Ordinary element | `.textContent` | Visible text inside the element |

Examples:

```js
const query = keywordInput.value.trim();
```

```js
const category = categorySelect.value;
```

---

## Filtering Logic

Each card must meet two conditions:

1. Its text contains the keyword.
2. Its category matches the selected category.

```js
const matchesKeyword = cardText.includes(query.toLowerCase());
```

```js
const matchesCategory =
  category === "all" || card.classList.contains(category);
```

The logical AND operator, `&&`, requires both conditions to be true.

```js
const shouldShow = matchesKeyword && matchesCategory;
```

| Keyword match | Category match | Card shown |
|---|---|---|
| `true` | `true` | Yes |
| `true` | `false` | No |
| `false` | `true` | No |
| `false` | `false` | No |

---

## Updating the Interface

The following statement adds or removes the `is-hidden` class.

```js
card.classList.toggle("is-hidden", !shouldShow);
```

- If `shouldShow` is `true`, the card remains visible.
- If `shouldShow` is `false`, JavaScript adds `is-hidden`.
- CSS hides any card that has the `is-hidden` class.

The status message updates with `textContent`.

```js
status.textContent = visibleCount + " activities found";
```

`textContent` should be used for plain text because it does not interpret the text as HTML.

---

# Functional Requirements

The completed application must satisfy all of the following requirements.

| ID | Requirement |
|---|---|
| FR-01 | Show all activities when the keyword is empty and All categories is selected. |
| FR-02 | Show only activities that match a keyword search. |
| FR-03 | Show zero activities and an accurate status message when no activity matches. |
| FR-04 | Show only activities that match the selected category. |
| FR-05 | Apply both keyword and category filters together. |
| FR-06 | Reset the keyword, category, visible cards, and status when Clear is clicked. |

---

# Test Cases

Perform the following checks before submitting the practical.

| Test | User action | Expected result |
|---|---|---|
| Initial state | Load the page | Six activities are visible. |
| Empty filter | Keep the keyword empty and choose All categories | All activities are visible. |
| Keyword filter | Enter `coding` and submit the form | Only Coding Workshop is visible. |
| No match | Enter `swimming` and submit the form | No cards are visible and the status reports `0 activities found`. |
| Category filter | Select Sports | Basketball Club and Badminton Training are visible. |
| Combined filter | Enter `robot`, select Technology, then submit | Only Robotics Challenge is visible. |
| Reset | Click Clear | The input is empty, category is All categories, and all activities are visible. |

---

# Debugging Guide

Debug the interaction in the same order that the application works.

## 1. Verify selection

```js
console.log(form);
console.log(keywordInput);
console.log(categorySelect);
console.log(clearButton);
console.log(status);
console.log(cards.length);
```

- A single selector should not return `null`.
- The NodeList length should match the number of activity cards.

## 2. Verify the event listener

```js
clearButton.addEventListener("click", function () {
  console.log("Clear button clicked");
});
```

Click the button and confirm the message appears in the browser Console.

## 3. Verify input values

```js
console.log(query);
console.log(category);
```

Confirm that JavaScript reads the expected keyword and selected category.

## 4. Verify the decision

```js
console.log(matchesKeyword);
console.log(matchesCategory);
console.log(shouldShow);
```

Check whether the Boolean values reflect the current card and filter choices.

## 5. Verify the update

Inspect a hidden card in Developer Tools. It should have the `is-hidden` class.

```html
<article class="activity-card sports is-hidden">
```

## 6. Verify the visible result

Count the displayed cards and compare the total with the status message.

---

# Extension Tasks

Complete these only after the core requirements work correctly.

## Live Search

Filter the activities as the user types.

```js
keywordInput.addEventListener("input", function () {
  const query = keywordInput.value.trim();
  const category = categorySelect.value;

  filterActivities(query, category);
});
```

## Clear Using Escape

Clear all filters when the user presses the Escape key.

```js
document.addEventListener("keydown", function (event) {
  if (event.key === "Escape") {
    keywordInput.value = "";
    categorySelect.value = "all";

    filterActivities("", "all");
  }
});
```

## Add an Activity

Create and append an additional activity card using:

- `document.createElement()`
- `textContent`
- `classList.add()`
- `append()`

---

# Reflection Questions

After completing the practical, answer the following questions in your own words:

1. Why is `querySelectorAll(".activity-card")` used instead of `querySelector(".activity-card")`?
2. Why is `forEach()` needed when filtering activities?
3. What is the difference between the `&&` and `||` operators?
4. Why does `classList.toggle("is-hidden", !shouldShow)` hide non-matching cards?
5. Why is `event.preventDefault()` needed for the form submission?
6. Which event would you choose for live filtering, and why?
