# 🎨 Complete HTML, CSS, SASS & Layout Notes for Interview

---

# 5. Semantic HTML & Browser Rendering

## 🖥️ How Browsers Render Pages

When you open a website, browser does a lot of work behind the scenes. Here is the complete step by step process:

### Step 1: HTML Parsing → DOM Tree
- Browser reads HTML file **top to bottom**
- Converts HTML tags into a tree structure called **DOM (Document Object Model)**
- Every HTML tag becomes a **node** in the tree

```
HTML Code:
<html>
  <body>
    <h1>Hello</h1>
    <p>World</p>
  </body>
</html>

DOM Tree:
html
 └── body
      ├── h1 → "Hello"
      └── p  → "World"
```

### Step 2: CSS Parsing → CSSOM Tree
- Browser reads all CSS (from `<style>` tags, external files, inline styles)
- Converts CSS into **CSSOM (CSS Object Model)** — a tree of styles
- Similar to DOM but for styles

```
CSS:
body { font-size: 16px; }
h1   { color: red; }

CSSOM Tree:
body → font-size: 16px
 └── h1 → color: red
```

### Step 3: Render Tree
- Browser **combines DOM + CSSOM** to create **Render Tree**
- Only **visible elements** are included (hidden elements like `display:none` are excluded)
- Each node in render tree has **content + styles**

```
DOM + CSSOM = Render Tree
(Only visible elements with their styles)
```

### Step 4: Layout (Reflow)
- Browser calculates **exact position and size** of every element
- Determines where each element goes on the screen
- Also called **"Layout"** step

### Step 5: Paint
- Browser **draws pixels** on the screen
- Fills colors, text, images, shadows, borders
- This is what you actually **see** on screen

### Step 6: Compositing
- If page has layers (animations, z-index), browser combines them
- **GPU** handles this step for smooth performance

### Complete Flow:
```
HTML File → DOM Tree
CSS File  → CSSOM Tree
             ↓
        Render Tree (DOM + CSSOM)
             ↓
           Layout (calculate positions)
             ↓
           Paint (draw pixels)
             ↓
        Compositing (combine layers)
             ↓
        ✅ Page visible on screen!
```

---

## ♻️ Understanding Reflow & Repaint

### Reflow (Layout Recalculation)
- Happens when **size or position** of an element changes
- Browser has to **recalculate layout** of all affected elements
- Very **expensive operation** — affects performance
- **Triggers Reflow:**
  - Changing `width`, `height`, `padding`, `margin`
  - Adding or removing elements from DOM
  - Changing font size
  - Resizing browser window

### Repaint
- Happens when **visual appearance** changes but **layout doesn't change**
- Browser redraws the element with new visual style
- **Less expensive** than reflow
- **Triggers Repaint:**
  - Changing `color`, `background-color`
  - Changing `visibility`
  - Changing `border-color`, `box-shadow`

### Relationship:
```
Reflow → always causes Repaint (because layout changed, need to redraw)
Repaint → does NOT always cause Reflow (only visual change)

Reflow = More expensive ❌
Repaint = Less expensive (but still avoid unnecessary ones)
```

### Why Layout Shifts Happen (CLS - Cumulative Layout Shift):
- When **images load without defined dimensions** — page jumps
- When **fonts load late** — text shifts
- When **ads or embeds load** — pushes content down
- This is bad for **user experience and SEO**

### How to Avoid Reflow & Repaint:
```javascript
// ❌ Bad - multiple reflows
element.style.width = '100px';
element.style.height = '200px';
element.style.margin = '10px';

// ✅ Good - one reflow using CSS class
element.classList.add('new-style');

// ✅ Good - use transform instead of position
// transform: translateX() → doesn't cause reflow
// left/top changes → causes reflow
```

---

## 🏗️ Semantic Tags — Architectural Explanation

### What is Semantic HTML?
- Semantic HTML means using tags that **describe their meaning/purpose**
- Instead of using `<div>` for everything, use meaningful tags
- Helps **browser, search engines, and screen readers** understand content

### Non-Semantic vs Semantic:
```html
<!-- ❌ Non-Semantic - no meaning -->
<div class="header">...</div>
<div class="navigation">...</div>
<div class="content">...</div>

<!-- ✅ Semantic - clear meaning -->
<header>...</header>
<nav>...</nav>
<main>...</main>
```

### Semantic Tags Explained:

#### `<header>`
- Represents **introductory content** of a page or section
- Contains: logo, navigation, site title
- Can appear multiple times (once for page, once inside article)
```html
<header>
  <img src="logo.png" alt="Logo">
  <nav>...</nav>
</header>
```

#### `<nav>`
- Contains **navigation links**
- Main menu, breadcrumbs, table of contents
```html
<nav>
  <a href="/home">Home</a>
  <a href="/about">About</a>
</nav>
```

#### `<main>`
- The **primary content** of the page
- Should only appear **ONCE** per page
- Screen readers jump directly to `<main>`
```html
<main>
  <!-- Main content of the page -->
</main>
```

#### `<section>`
- A **thematic grouping** of content
- Represents a **chapter or part** of a page
- Should have a heading inside
```html
<section>
  <h2>Our Services</h2>
  <p>...</p>
</section>
```

#### `<article>`
- **Self-contained content** that can stand alone
- Makes sense on its own — like a blog post, news article, comment
- Can be shared or syndicated independently
```html
<article>
  <h2>Blog Post Title</h2>
  <p>Content...</p>
</article>
```

#### `<aside>`
- Content **related to main content** but not central
- Sidebar, related articles, advertisements, quotes
```html
<aside>
  <h3>Related Articles</h3>
  <ul>...</ul>
</aside>
```

#### `<footer>`
- **Bottom section** of a page or section
- Contains: copyright, links, contact info, social media
```html
<footer>
  <p>&copy; 2024 My Website</p>
</footer>
```

### Complete Page Architecture:
```html
<body>
  <header>          <!-- Site header: logo + nav -->
    <nav>...</nav>
  </header>

  <main>            <!-- Primary content (only one per page) -->
    <section>       <!-- Grouped thematic content -->
      <article>     <!-- Self-contained piece of content -->
        ...
      </article>
    </section>

    <aside>         <!-- Sidebar / related content -->
      ...
    </aside>
  </main>

  <footer>          <!-- Site footer -->
    ...
  </footer>
</body>
```

---

## 🔍 SEO Impact of Semantic HTML

### Why Semantic HTML Improves Search Ranking:

- **Search engines (Google)** crawl your HTML to understand page content
- Semantic tags tell Google **what is important** on your page
- Better understanding = **better indexing** = **higher ranking**

### Specific Benefits:

| Semantic Element | SEO Benefit |
|-----------------|-------------|
| `<h1>` | Main keyword signal to Google |
| `<article>` | Identifies standalone content worth indexing |
| `<nav>` | Helps Google understand site structure |
| `<header>` | Identifies page identity section |
| `<main>` | Tells Google where main content is |
| `alt` in images | Image search ranking |

### Key Points:
- ✅ Google **rewards structured, readable HTML**
- ✅ Semantic HTML helps with **rich snippets** in search results
- ✅ Better **crawlability** — Google bot understands page faster
- ✅ Reduces **duplicate content confusion**
- ✅ Works with **schema markup** for enhanced search results

---

## ♿ Accessibility Basics — ARIA Roles & Inclusive Design

### What is Accessibility (a11y)?
- Making websites **usable by everyone** — including people with disabilities
- Disabilities: visual, hearing, motor, cognitive
- Screen readers, keyboard navigation, voice control

### ARIA (Accessible Rich Internet Applications)
- A set of **HTML attributes** that add extra meaning to elements
- Helps **screen readers** understand dynamic content
- Use ARIA **only when needed** — semantic HTML is better when possible

### Common ARIA Attributes:

#### `role` — Defines what an element is
```html
<div role="button">Click Me</div>
<div role="navigation">...</div>
<div role="alert">Error message!</div>
```

#### `aria-label` — Gives a text label to element
```html
<!-- Icon button with no visible text -->
<button aria-label="Close menu">✕</button>
```

#### `aria-hidden` — Hides element from screen readers
```html
<!-- Decorative icon, screen reader should ignore -->
<span aria-hidden="true">🌟</span>
```

#### `aria-expanded` — Shows if element is open/closed
```html
<button aria-expanded="false">Menu</button>
```

#### `aria-describedby` — Links element to its description
```html
<input aria-describedby="email-hint">
<p id="email-hint">Enter your email address</p>
```

### Inclusive Design Principles:
- ✅ **Keyboard navigable** — everything works with Tab key
- ✅ **Color contrast** — text readable for color blind users
- ✅ **Alt text** on images — screen readers can describe images
- ✅ **Focus indicators** — visible outline when tabbing
- ✅ **Skip links** — "Skip to main content" for keyboard users
- ✅ **Descriptive link text** — "Read more about CSS" not just "Click here"

---

## 📑 Content Structure — Heading Hierarchy

### Why Heading Hierarchy Matters:
- Creates a **logical document outline**
- Helps **SEO** — `h1` is most important keyword signal
- Helps **screen readers** navigate page by headings
- Makes content **readable and scannable**

### Rules:
- Only **ONE `<h1>`** per page (main topic)
- Don't **skip heading levels** (don't go from h2 to h4)
- Headings should be **nested logically**

```html
<!-- ✅ Correct Hierarchy -->
<h1>Web Development Guide</h1>          <!-- Page Title -->
  <h2>HTML Basics</h2>                  <!-- Main Section -->
    <h3>Semantic HTML</h3>              <!-- Sub-section -->
      <h4>Header Tag</h4>               <!-- Detail -->
  <h2>CSS Basics</h2>                   <!-- Main Section -->
    <h3>Flexbox</h3>                    <!-- Sub-section -->

<!-- ❌ Wrong - Skipping levels -->
<h1>Title</h1>
<h3>Skipped h2!</h3>   <!-- Bad! -->
```

---

## 📋 Lists — Ordered & Unordered

### Unordered List `<ul>`
- Items with **no specific order** (bullets)
- Use when order **doesn't matter**
```html
<ul>
  <li>HTML</li>
  <li>CSS</li>
  <li>JavaScript</li>
</ul>
```
Output: • HTML • CSS • JavaScript

### Ordered List `<ol>`
- Items with **specific order** (numbers)
- Use when **sequence matters** (steps, rankings)
```html
<ol>
  <li>Open browser</li>
  <li>Type URL</li>
  <li>Press Enter</li>
</ol>
```
Output: 1. Open browser  2. Type URL  3. Press Enter

### Definition List `<dl>`
- For **terms and definitions**
```html
<dl>
  <dt>HTML</dt>
  <dd>HyperText Markup Language</dd>
  <dt>CSS</dt>
  <dd>Cascading Style Sheets</dd>
</dl>
```

### Nested Lists:
```html
<ul>
  <li>Frontend
    <ul>
      <li>HTML</li>
      <li>CSS</li>
    </ul>
  </li>
  <li>Backend</li>
</ul>
```

---

## 🖼️ Responsive Images — Basic Image Optimization

### Why Image Optimization Matters:
- Images are the **biggest files** on most websites
- Large images = **slow loading** = bad user experience + bad SEO
- Different devices need **different image sizes**

### `srcset` — Serve Different Image Sizes:
```html
<!-- Browser picks the right size based on screen width -->
<img
  src="image-400.jpg"
  srcset="image-400.jpg 400w,
          image-800.jpg 800w,
          image-1200.jpg 1200w"
  alt="A beautiful sunset"
>
```

### `sizes` — Tell Browser How Big Image Will Display:
```html
<img
  srcset="small.jpg 400w, large.jpg 800w"
  sizes="(max-width: 600px) 100vw, 50vw"
  src="large.jpg"
  alt="Example image"
>
```

### `loading="lazy"` — Lazy Loading:
```html
<!-- Image only loads when user scrolls to it -->
<img src="image.jpg" loading="lazy" alt="Example">
```

### Modern Image Formats:
- **WebP** — 30% smaller than JPEG, same quality
- **AVIF** — Even smaller than WebP (newest)
- **SVG** — For icons and logos (scales perfectly)

### `<picture>` — Serve Different Formats:
```html
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Fallback">   <!-- Fallback -->
</picture>
```

### Best Practices:
- ✅ Always add **`alt` attribute**
- ✅ Define **`width` and `height`** (prevents layout shift)
- ✅ Use **lazy loading** for images below the fold
- ✅ Use modern formats (WebP, AVIF)
- ✅ Compress images before uploading

---

## 🔗 Links & Navigation Flow

### Internal vs External Links:

#### Internal Links
- Links that go to **another page on the same website**
- Help users **navigate your site**
- Improve **SEO** by spreading link value across pages
```html
<a href="/about">About Us</a>
<a href="/blog/post-1">Read Our Blog</a>
<a href="#section-2">Jump to Section 2</a>  <!-- anchor link -->
```

#### External Links
- Links that go to a **different website**
- Use `target="_blank"` to open in new tab
- Use `rel="noopener noreferrer"` for **security**
```html
<a href="https://google.com"
   target="_blank"
   rel="noopener noreferrer">
  Visit Google
</a>
```

### Why `rel="noopener noreferrer"`?
- `noopener` — prevents new tab from accessing your page (security)
- `noreferrer` — hides referrer info from destination site (privacy)

### Navigation Best Practices:
- ✅ Use **`<nav>`** for main navigation
- ✅ **Descriptive link text** (not "click here")
- ✅ **Current page** should be marked active
- ✅ Keyboard accessible navigation

---

## 📝 Forms Introduction

### Basic Form Structure:
```html
<form action="/submit" method="POST">
  <!-- form elements go here -->
</form>
```
- `action` — where to send form data
- `method` — GET (visible in URL) or POST (hidden in body)

### Input Types:
```html
<!-- Text inputs -->
<input type="text"     placeholder="Your name">
<input type="email"    placeholder="email@example.com">
<input type="password" placeholder="Password">
<input type="number"   min="1" max="100">
<input type="tel"      placeholder="Phone number">
<input type="url"      placeholder="https://...">

<!-- Selection inputs -->
<input type="checkbox"> Remember me
<input type="radio"   name="gender" value="male"> Male
<input type="range"   min="0" max="100">

<!-- Special inputs -->
<input type="date">
<input type="file">
<input type="color">
<input type="search" placeholder="Search...">
<input type="hidden" value="secret-data">

<!-- Submit -->
<input type="submit" value="Submit">
<button type="submit">Submit</button>
```

### Form Validation Basics:

#### HTML5 Built-in Validation (no JavaScript needed!):
```html
<form>
  <!-- required — field must be filled -->
  <input type="text" required>

  <!-- minlength/maxlength — character limits -->
  <input type="text" minlength="3" maxlength="50">

  <!-- min/max — number range -->
  <input type="number" min="18" max="100">

  <!-- pattern — must match regex pattern -->
  <input type="text" pattern="[A-Za-z]{3,}">

  <!-- email type auto-validates email format -->
  <input type="email" required>
</form>
```

### Complete Form Example:
```html
<form action="/register" method="POST">
  <label for="name">Full Name</label>
  <input type="text" id="name" name="name" required minlength="2">

  <label for="email">Email Address</label>
  <input type="email" id="email" name="email" required>

  <label for="password">Password</label>
  <input type="password" id="password" name="password" required minlength="8">

  <label for="age">Age</label>
  <input type="number" id="age" name="age" min="18" max="100">

  <label for="country">Country</label>
  <select id="country" name="country">
    <option value="">Select Country</option>
    <option value="in">India</option>
    <option value="us">USA</option>
  </select>

  <label for="message">Message</label>
  <textarea id="message" name="message" rows="4"></textarea>

  <button type="submit">Register</button>
</form>
```

### Important: Always use `<label>` with inputs!
```html
<!-- ✅ Correct - label connected with for + id -->
<label for="username">Username</label>
<input type="text" id="username">

<!-- ❌ Wrong - no connection -->
<p>Username</p>
<input type="text">
```

---
---

# 6. CSS Core Fundamentals

## 🎯 CSS Syntax & Selectors

### CSS Basic Syntax:
```css
selector {
  property: value;
}
```

### Types of Selectors:

#### Element Selector
- Selects **all elements** of that type
```css
p { color: blue; }      /* All <p> elements */
h1 { font-size: 32px; } /* All <h1> elements */
```

#### Class Selector (`.`)
- Selects elements with **that class**
- Can be used on **multiple elements**
```css
.button { background: blue; }
.card   { border: 1px solid; }
```
```html
<div class="card">...</div>
<p class="card">...</p>
```

#### ID Selector (`#`)
- Selects element with **that specific ID**
- ID must be **unique** on a page
- **Higher specificity** than class
```css
#header { height: 60px; }
#logo   { width: 100px; }
```

#### Grouping Selector (`,`)
- Apply **same styles to multiple selectors**
```css
h1, h2, h3 { font-family: Arial; }
.card, .box { border-radius: 8px; }
```

#### Descendant Selector (space)
- Selects elements **inside another element**
```css
/* Select all <p> inside .container */
.container p { color: red; }

/* Select all <a> inside <nav> */
nav a { text-decoration: none; }
```

#### Child Selector (`>`)
- Selects **direct children only** (not grandchildren)
```css
/* Only direct <li> children of <ul> */
ul > li { list-style: none; }
```

#### Adjacent Sibling (`+`)
- Selects **immediately next sibling**
```css
/* <p> immediately after <h2> */
h2 + p { font-size: 18px; }
```

#### General Sibling (`~`)
- Selects **all following siblings**
```css
h2 ~ p { color: gray; }
```

#### Attribute Selector
```css
input[type="email"]  { border: 1px solid blue; }
a[href^="https"]     { color: green; }  /* starts with */
a[href$=".pdf"]      { color: red; }    /* ends with */
```

#### Pseudo-class Selectors
```css
a:hover        { color: red; }      /* when mouse hovers */
input:focus    { outline: blue; }   /* when focused */
li:first-child { font-weight: bold; }
li:last-child  { margin: 0; }
li:nth-child(2){ color: red; }      /* 2nd child */
li:nth-child(odd)  { background: #eee; }
```

#### Pseudo-element Selectors
```css
p::first-line   { font-weight: bold; }
p::first-letter { font-size: 2em; }
.btn::before    { content: "→"; }   /* add content before */
.btn::after     { content: "←"; }   /* add content after */
```

#### Universal Selector (`*`)
```css
* { box-sizing: border-box; margin: 0; padding: 0; }
```

---

## ⚖️ Specificity & Cascade

### What is Specificity?
- When **multiple CSS rules** target the same element, specificity decides **which rule wins**
- Higher specificity = **that style is applied**

### Specificity Calculation:

| Selector | Points |
|----------|--------|
| Inline style | 1000 |
| ID (`#id`) | 100 |
| Class, attribute, pseudo-class | 10 |
| Element, pseudo-element | 1 |
| Universal (`*`) | 0 |

### Examples:
```css
p               { color: black; }   /* 1 point */
.text           { color: blue; }    /* 10 points */
#heading        { color: green; }   /* 100 points */
style="color:red"                   /* 1000 points */
```

```css
/* Specificity comparison */
p              { color: black; }  /* 0-0-1 = 1 */
.card p        { color: blue; }   /* 0-1-1 = 11 */
#main .card p  { color: red; }    /* 1-1-1 = 111 */
```

### What is Cascade?
- CSS stands for **Cascading** Style Sheets
- "Cascade" means styles **flow down** from multiple sources
- When specificity is equal, **last defined rule wins**

### Order of Cascade (Low → High Priority):
```
1. Browser default styles
2. External CSS file
3. Internal CSS (<style> tag)
4. Inline CSS (style="...")
5. !important (overrides everything ⚠️)
```

### `!important`:
```css
p { color: blue !important; }  /* Overrides everything */
```
> ⚠️ Avoid `!important` — it makes debugging very hard

---

## 🌊 Inheritance & Computed Styles

### What is Inheritance?
- Some CSS properties are **automatically passed from parent to child**
- Child elements **inherit** styles from their parent

### Properties that ARE Inherited (Text-related):
```css
/* These flow down to children automatically */
color, font-size, font-family, font-weight,
line-height, text-align, visibility, cursor
```

### Properties that are NOT Inherited (Layout-related):
```css
/* These do NOT flow to children */
margin, padding, border, width, height,
background, display, position
```

### Example:
```html
<div style="color: red; border: 1px solid blue;">
  <p>This text is RED (inherited color)</p>
  <!-- But border does NOT inherit -->
</div>
```

### Controlling Inheritance:
```css
/* Force inheritance */
.child { color: inherit; }

/* Reset to browser default */
.child { color: initial; }

/* Use parent's computed value */
.child { color: unset; }
```

### Computed Styles:
- **Computed styles** are the **final calculated values** of all CSS properties
- After browser applies all cascade, specificity, and inheritance
- Can view in **browser DevTools → Computed tab**

```css
/* You write */
font-size: 1.5em;

/* Browser computes (if parent is 16px) */
font-size: 24px;  /* This is the computed value */
```

---

## 📦 Box Model — Deep Understanding

### Every HTML Element is a Box!
```
┌─────────────────────────────────┐
│           MARGIN                │  (outside, transparent)
│  ┌───────────────────────────┐  │
│  │         BORDER            │  │  (border line)
│  │  ┌─────────────────────┐  │  │
│  │  │      PADDING        │  │  │  (inside space)
│  │  │  ┌───────────────┐  │  │  │
│  │  │  │    CONTENT    │  │  │  │  (actual content)
│  │  │  └───────────────┘  │  │  │
│  │  └─────────────────────┘  │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
```

### Parts of Box Model:

#### Content
- The **actual content** (text, image, video)
- Size controlled by `width` and `height`

#### Padding
- **Space between content and border** (inside)
- Padding is **same color as background**
- Makes element **bigger**
```css
padding: 20px;             /* all sides */
padding: 10px 20px;        /* top/bottom left/right */
padding: 10px 20px 30px;   /* top left/right bottom */
padding: 5px 10px 15px 20px; /* top right bottom left */
```

#### Border
- **Line around the padding**
```css
border: 2px solid red;
border: 1px dashed blue;
border-radius: 8px;         /* rounded corners */
border-radius: 50%;         /* circle */
```

#### Margin
- **Space OUTSIDE the border** (between elements)
- Transparent — shows parent's background
```css
margin: 20px;
margin: 0 auto;  /* center element horizontally */
```

### `box-sizing` Property (VERY IMPORTANT):

#### `content-box` (default — confusing!):
```css
.box {
  box-sizing: content-box;  /* default */
  width: 300px;
  padding: 20px;
  border: 5px solid;
  /* TOTAL width = 300 + 20+20 + 5+5 = 350px */
  /* Padding and border ADD to total size */
}
```

#### `border-box` (better — what you expect!):
```css
.box {
  box-sizing: border-box;
  width: 300px;
  padding: 20px;
  border: 5px solid;
  /* TOTAL width = 300px exactly */
  /* Padding and border are INCLUDED in width */
}

/* Best practice — apply to everything */
* {
  box-sizing: border-box;
}
```

### Margin Collapse:
- When **two vertical margins meet**, they collapse into one
- The **larger margin wins**
```css
/* Two paragraphs: margin-bottom: 20px and margin-top: 30px */
/* Don't add to 50px — they collapse to 30px (the larger one) */
p { margin: 20px 0; }
h2 { margin-bottom: 30px; }
/* Space between h2 and p = 30px, not 50px */
```

---

## 📏 CSS Units

### Absolute Units:
```css
px  /* pixels — fixed size, most common */
pt  /* points — used in print */
cm, mm, in  /* physical units */
```

### Relative Units:

#### `em` — Relative to Parent's font-size
```css
/* If parent font-size = 16px */
.child {
  font-size: 2em;   /* 2 × 16px = 32px */
  padding: 1em;     /* 1 × 32px = 32px (relative to THIS element's font-size) */
}
/* Problem: em compounds! nested ems get confusing */
```

#### `rem` — Relative to Root (`<html>`) font-size
```css
html { font-size: 16px; }  /* root font size */

.element {
  font-size: 1.5rem;  /* 1.5 × 16px = 24px (always!) */
  padding: 2rem;      /* 2 × 16px = 32px */
}
/* rem is predictable — always based on root, not parent */
```

#### `%` — Relative to Parent
```css
.parent { width: 800px; }
.child  { width: 50%; }  /* 50% of 800px = 400px */
```

#### `vw` & `vh` — Viewport Width & Height
```css
/* vw = 1% of viewport width */
/* vh = 1% of viewport height */
.hero { height: 100vh; }  /* full screen height */
.banner { width: 50vw; }  /* half screen width */
```

### When to Use What:

| Unit | Best For |
|------|----------|
| `px` | Borders, shadows, fixed sizes |
| `rem` | Font sizes, spacing (consistent) |
| `em` | Component-specific spacing |
| `%` | Widths relative to parent |
| `vw/vh` | Full-screen sections, hero areas |

---

## 🔧 Modern CSS Functions — `clamp()`, `min()`, `max()`

### `min()` — Picks the SMALLEST value
```css
/* Use whichever is smaller */
.box { width: min(500px, 100%); }
/* On large screen: 500px */
/* On small screen: 100% (which is less than 500px) */
```

### `max()` — Picks the LARGEST value
```css
/* Use whichever is larger */
.box { width: max(300px, 50%); }
/* Minimum width is always 300px */
```

### `clamp()` — Clamps value between min and max
```css
/* clamp(minimum, preferred, maximum) */
.text { font-size: clamp(14px, 4vw, 24px); }
/*
  - Never smaller than 14px
  - Grows with viewport (4vw = 4% of viewport width)
  - Never larger than 24px
  → Perfect fluid/responsive typography!
*/

.container { width: clamp(320px, 80%, 1200px); }
/* Min: 320px, Preferred: 80% of screen, Max: 1200px */
```

---

## 🎨 CSS Variables — Using `:root` for Global Theming

### What are CSS Variables?
- Also called **Custom Properties**
- Store **reusable values** that can be used anywhere in CSS
- Defined with `--` prefix
- Accessed with `var()` function

### Defining Variables in `:root`:
```css
/* :root = the <html> element — highest scope */
:root {
  /* Colors */
  --color-primary:    #6366f1;
  --color-secondary:  #f59e0b;
  --color-background: #ffffff;
  --color-text:       #1f2937;
  --color-error:      #ef4444;

  /* Typography */
  --font-size-sm:  0.875rem;  /* 14px */
  --font-size-md:  1rem;      /* 16px */
  --font-size-lg:  1.25rem;   /* 20px */
  --font-size-xl:  1.5rem;    /* 24px */

  /* Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 48px;

  /* Border radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;
  --radius-full: 9999px;

  /* Shadows */
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.1);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.1);
}
```

### Using Variables:
```css
.button {
  background-color: var(--color-primary);
  padding: var(--space-sm) var(--space-md);
  border-radius: var(--radius-md);
  font-size: var(--font-size-md);
  box-shadow: var(--shadow-sm);
}

.card {
  background: var(--color-background);
  padding: var(--space-lg);
  border-radius: var(--radius-lg);
}
```

### Dark Mode with CSS Variables:
```css
:root {
  --bg-color: #ffffff;
  --text-color: #000000;
}

[data-theme="dark"] {
  --bg-color: #1a1a1a;
  --text-color: #ffffff;
}

body {
  background: var(--bg-color);
  color: var(--text-color);
}
/* Now toggle dark mode by just changing data-theme attribute! */
```

### Fallback Values:
```css
/* If --color-primary doesn't exist, use blue as fallback */
.button { color: var(--color-primary, blue); }
```

---

## 🏛️ Design System & Design Tokens

### What is a Design System?
- A **collection of reusable components, rules and standards** for building consistent UIs
- Ensures **visual consistency** across entire application
- Examples: Material Design (Google), Ant Design, Chakra UI

### What are Design Tokens?
- Design tokens are the **smallest building blocks** of a design system
- Named variables that store **visual design decisions**
- Think of them as **"named values for design properties"**
- Can be shared across platforms (Web, iOS, Android)

### Types of Design Tokens:

#### Color Tokens:
```css
:root {
  /* Primitive tokens (raw values) */
  --blue-50:  #eff6ff;
  --blue-100: #dbeafe;
  --blue-500: #3b82f6;
  --blue-900: #1e3a8a;

  /* Semantic tokens (meaningful names) */
  --color-brand-primary:   var(--blue-500);
  --color-interactive:     var(--blue-500);
  --color-text-primary:    #111827;
  --color-text-secondary:  #6b7280;
  --color-border:          #e5e7eb;
  --color-success:         #22c55e;
  --color-warning:         #f59e0b;
  --color-danger:          #ef4444;
}
```

#### Spacing Tokens:
```css
:root {
  /* Base unit: 4px */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-6: 24px;
  --space-8: 32px;
  --space-12: 48px;
  --space-16: 64px;
}
```

#### Typography Tokens:
```css
:root {
  --font-family-base:    'Inter', sans-serif;
  --font-family-heading: 'Poppins', sans-serif;
  --font-family-mono:    'Fira Code', monospace;

  --font-size-xs:   0.75rem;   /* 12px */
  --font-size-sm:   0.875rem;  /* 14px */
  --font-size-base: 1rem;      /* 16px */
  --font-size-lg:   1.125rem;  /* 18px */
  --font-size-xl:   1.25rem;   /* 20px */
  --font-size-2xl:  1.5rem;    /* 24px */
  --font-size-3xl:  1.875rem;  /* 30px */
  --font-size-4xl:  2.25rem;   /* 36px */

  --font-weight-normal:   400;
  --font-weight-medium:   500;
  --font-weight-semibold: 600;
  --font-weight-bold:     700;

  --line-height-tight:  1.25;
  --line-height-normal: 1.5;
  --line-height-loose:  2;
}
```

### Benefits of Design Tokens:
- ✅ **Consistency** — same values everywhere
- ✅ **Easy theming** — change one variable, updates everywhere
- ✅ **Team collaboration** — designers and developers speak same language
- ✅ **Scalable** — easy to maintain large projects

---
---

# 7. Working With SASS (SCSS)

## 📦 What is SASS?

- SASS (**Syntactically Awesome Style Sheets**) is a **CSS preprocessor**
- It adds **powerful features** to CSS that CSS doesn't have natively
- You write SASS/SCSS code → it **compiles into regular CSS**
- Makes CSS more **maintainable, organized, and powerful**

### SASS vs SCSS:

| Feature | SASS (old syntax) | SCSS (new syntax) |
|---------|------------------|------------------|
| File extension | `.sass` | `.scss` |
| Syntax | No curly braces, no semicolons | Uses `{}` and `;` like CSS |
| CSS compatible | No | Yes (valid CSS = valid SCSS) |
| Popular? | Less popular | More popular ✅ |

### SASS Syntax (old):
```sass
/* .sass file */
$primary: blue

.button
  background: $primary
  padding: 10px
```

### SCSS Syntax (recommended):
```scss
/* .scss file */
$primary: blue;

.button {
  background: $primary;
  padding: 10px;
}
```

> **Use SCSS** — it's more popular and valid CSS works inside it

---

## ⚙️ Setting Up SCSS Environment

### Method 1: Using Node.js (Recommended)
```bash
# Install Node.js first (nodejs.org)

# Install Sass globally
npm install -g sass

# Compile SCSS to CSS
sass input.scss output.css

# Watch mode (auto-compile on save)
sass --watch input.scss:output.css

# Watch entire folder
sass --watch scss/:css/
```

### Method 2: VS Code Extension
- Install **"Live Sass Compiler"** extension in VS Code
- Click **"Watch Sass"** at bottom of VS Code
- Auto-compiles on save ✅

### Project Structure:
```
project/
├── scss/
│   ├── main.scss
│   ├── _variables.scss
│   ├── _mixins.scss
│   └── components/
│       ├── _button.scss
│       └── _card.scss
├── css/
│   └── main.css  (auto-generated)
└── index.html
```

---

## 📝 SCSS Variables

```scss
// Define variables with $ prefix
$primary-color:    #6366f1;
$secondary-color:  #f59e0b;
$font-size-base:   16px;
$font-family:      'Inter', sans-serif;
$border-radius:    8px;
$spacing-unit:     8px;

// Use variables
.button {
  background: $primary-color;
  font-size: $font-size-base;
  border-radius: $border-radius;
  padding: $spacing-unit * 2;  // Can use math!
}

.card {
  border: 1px solid $secondary-color;
  font-family: $font-family;
}
```

### Advantage over CSS variables:
- SCSS variables are **compile-time** (processed before browser)
- Can be used in **calculations and conditions**
- CSS variables are **runtime** (live in browser, more flexible)

---

## 🪆 SCSS Nesting

- Write CSS in a **hierarchical structure** matching HTML structure
- Avoids **repeating parent selectors**

```scss
// ❌ Regular CSS - repetitive
nav { background: #333; }
nav ul { list-style: none; }
nav ul li { display: inline; }
nav ul li a { color: white; }
nav ul li a:hover { color: yellow; }

// ✅ SCSS Nesting - clean!
nav {
  background: #333;

  ul {
    list-style: none;

    li {
      display: inline;

      a {
        color: white;

        &:hover {         // & = parent selector (nav ul li a)
          color: yellow;
        }
      }
    }
  }
}
```

### `&` Parent Selector:
```scss
.button {
  background: blue;

  &:hover { background: darkblue; }     // .button:hover
  &:focus { outline: 2px solid blue; }  // .button:focus
  &:disabled { opacity: 0.5; }          // .button:disabled

  &--primary { background: blue; }      // .button--primary (BEM)
  &--secondary { background: gray; }    // .button--secondary

  &.is-active { border: 2px solid; }    // .button.is-active
}
```

### ⚠️ Don't Over-Nest!
```scss
// ❌ Too deep - creates overly specific CSS
.nav .nav-list .nav-item .nav-link span { ... }

// ✅ Max 3 levels deep
.nav {
  .nav-item {
    .nav-link { ... }
  }
}
```

---

## 📂 SCSS Partials and Imports

### Partials
- SCSS files starting with `_` (underscore) are **partials**
- They are **not compiled** into their own CSS file
- Must be **imported** into a main file
- Used to **organize code** into smaller files

```
scss/
├── _variables.scss    ← partial (won't compile alone)
├── _mixins.scss       ← partial
├── _reset.scss        ← partial
├── _typography.scss   ← partial
├── components/
│   ├── _button.scss   ← partial
│   └── _card.scss     ← partial
└── main.scss          ← main file (will compile to main.css)
```

### Importing Partials (Old way - `@import`):
```scss
// main.scss
@import 'variables';   // No need for _ or .scss
@import 'mixins';
@import 'reset';
@import 'typography';
@import 'components/button';
@import 'components/card';
```

### Modern way — `@use` and `@forward`:
```scss
// main.scss (modern approach)
@use 'variables' as v;
@use 'mixins' as m;
@use 'components/button';

// Access variables with namespace
.element {
  color: v.$primary-color;
}
```

---

## 🔁 SCSS Mixins

- A **reusable block of CSS** you can include anywhere
- Like a **function** that outputs CSS
- Can accept **arguments** (parameters)

### Basic Mixin:
```scss
// Define mixin
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

// Use mixin with @include
.hero {
  @include flex-center;
  height: 100vh;
}

.card {
  @include flex-center;
}
```

### Mixin with Parameters:
```scss
// Define with parameters
@mixin button-style($bg-color, $text-color: white) {
  //                            ↑ default value
  background-color: $bg-color;
  color: $text-color;
  padding: 10px 20px;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  &:hover {
    background-color: darken($bg-color, 10%);
  }
}

// Use with different values
.btn-primary   { @include button-style(#6366f1); }
.btn-secondary { @include button-style(#f59e0b, black); }
.btn-danger    { @include button-style(#ef4444); }
```

### Useful Mixin Examples:
```scss
// Media query mixin
@mixin responsive($breakpoint) {
  @if $breakpoint == mobile {
    @media (max-width: 768px) { @content; }
  } @else if $breakpoint == tablet {
    @media (max-width: 1024px) { @content; }
  } @else if $breakpoint == desktop {
    @media (min-width: 1025px) { @content; }
  }
}

// Usage
.container {
  width: 100%;

  @include responsive(mobile) {
    padding: 10px;
  }
  @include responsive(desktop) {
    max-width: 1200px;
    margin: 0 auto;
  }
}

// Flexbox mixin
@mixin flex($direction: row, $justify: flex-start, $align: stretch, $wrap: nowrap) {
  display: flex;
  flex-direction: $direction;
  justify-content: $justify;
  align-items: $align;
  flex-wrap: $wrap;
}

.navbar  { @include flex(row, space-between, center); }
.sidebar { @include flex(column, flex-start, stretch); }
```

---

## 🧬 SCSS Inheritance / Extends (`@extend`)

- Shares CSS properties between selectors
- Avoid repeating same styles

```scss
// Define a base style
%button-base {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

// Extend it (inherit the styles)
.btn-primary {
  @extend %button-base;
  background: blue;
  color: white;
}

.btn-secondary {
  @extend %button-base;
  background: gray;
  color: white;
}

.btn-outline {
  @extend %button-base;
  background: transparent;
  border: 2px solid blue;
  color: blue;
}
```

### `%placeholder` vs Regular Class:
```scss
// % placeholder - only outputs CSS when extended
// (doesn't create .button-base class in output)
%button-base { ... }

// Regular class - always in output even if not extended
.button-base { ... }
```

### Mixin vs Extend:

| Feature | Mixin | Extend |
|---------|-------|--------|
| Parameters | ✅ Yes | ❌ No |
| Code duplication | Each include copies code | Selectors are grouped |
| Use when | Dynamic values needed | Sharing identical styles |

---

## 🔢 SCSS Functions

### Built-in Functions:
```scss
// Color functions
darken($color, 10%)    // make color 10% darker
lighten($color, 10%)   // make color 10% lighter
rgba($color, 0.5)      // add transparency
mix(blue, red, 50%)    // mix two colors

// Math functions
percentage(0.5)        // 50%
round(4.7)             // 5
ceil(4.1)              // 5
floor(4.9)             // 4
abs(-10)               // 10
```

### Custom Functions:
```scss
// Define function with @function
@function rem($px) {
  @return $px / 16px * 1rem;
}

@function spacing($multiplier) {
  @return $multiplier * 8px;
}

// Use function
.element {
  font-size: rem(24px);    // → 1.5rem
  padding: spacing(2);     // → 16px
  margin: spacing(3);      // → 24px
}
```

---

## ➕ SCSS Operators

```scss
// Math operators in SCSS
.box {
  width: 100px + 50px;    // 150px
  height: 200px - 50px;   // 150px
  margin: 10px * 2;        // 20px
  padding: 20px / 2;       // 10px
  font-size: 10px + 2 * 3; // 16px
}

// Percentage calculations
.grid-item {
  $total: 3;
  width: (100% / $total);  // 33.33%
}
```

---

## 🎛️ Advanced SCSS — Control Directives

### `@if`, `@else if`, `@else`:
```scss
@mixin text-size($size) {
  @if $size == small {
    font-size: 12px;
  } @else if $size == medium {
    font-size: 16px;
  } @else if $size == large {
    font-size: 24px;
  } @else {
    font-size: $size;  // Use the value directly
  }
}

.small-text  { @include text-size(small); }
.medium-text { @include text-size(medium); }
.custom-text { @include text-size(18px); }
```

### `@for` Loop:
```scss
// Generate utility classes
@for $i from 1 through 5 {
  .mt-#{$i} { margin-top: $i * 8px; }
  .mb-#{$i} { margin-bottom: $i * 8px; }
  .p-#{$i}  { padding: $i * 8px; }
}

// Output:
// .mt-1 { margin-top: 8px; }
// .mt-2 { margin-top: 16px; }
// .mt-3 { margin-top: 24px; }
// .mt-4 { margin-top: 32px; }
// .mt-5 { margin-top: 40px; }
```

### `@each` Loop:
```scss
$colors: (
  primary:   #6366f1,
  secondary: #f59e0b,
  success:   #22c55e,
  danger:    #ef4444,
  warning:   #f97316
);

@each $name, $color in $colors {
  .text-#{$name}       { color: $color; }
  .bg-#{$name}         { background-color: $color; }
  .border-#{$name}     { border-color: $color; }
}

// Output:
// .text-primary { color: #6366f1; }
// .bg-primary { background-color: #6366f1; }
// etc...
```

### `@while` Loop:
```scss
$size: 1;
@while $size <= 5 {
  .icon-#{$size * 10} {
    width: $size * 10px;
    height: $size * 10px;
  }
  $size: $size + 1;
}
```

---

## 🎨 SCSS Color Functions

```scss
$base-color: #6366f1;

.palette {
  // Lighten / Darken
  color-light:  lighten($base-color, 20%);   // 20% lighter
  color-dark:   darken($base-color, 20%);    // 20% darker

  // Saturate / Desaturate
  color-vivid:  saturate($base-color, 20%);
  color-muted:  desaturate($base-color, 20%);

  // Transparency
  color-transparent: rgba($base-color, 0.5);
  color-opaque: transparentize($base-color, 0.3);

  // Complement
  color-opposite: complement($base-color);

  // Mix colors
  color-mixed: mix($base-color, white, 30%);
  // 30% base-color + 70% white
}

// Practical usage
.button {
  background: $base-color;

  &:hover  { background: darken($base-color, 10%); }
  &:active { background: darken($base-color, 20%); }
  &:focus  {
    box-shadow: 0 0 0 3px rgba($base-color, 0.3);
  }
}
```

---
---

# 8. CSS Layout Mastery

## 💪 Flexbox Deep Dive

### What is Flexbox?
- A **1D layout system** — works in one direction (row OR column)
- Perfect for **navigation bars, card rows, centering elements**
- Parent becomes **flex container**, children become **flex items**

```css
.container {
  display: flex;  /* Make it a flex container */
}
```

### Main Axis vs Cross Axis:
```
flex-direction: row (default)
Main Axis:   →→→→→→→→→→ (horizontal)
Cross Axis:  ↓↓↓↓↓↓↓↓↓↓ (vertical)

flex-direction: column
Main Axis:   ↓↓↓↓↓↓↓↓↓↓ (vertical)
Cross Axis:  →→→→→→→→→→ (horizontal)
```

### Container Properties (Parent):

```css
.container {
  display: flex;

  /* Direction */
  flex-direction: row;           /* → left to right (default) */
  flex-direction: row-reverse;   /* ← right to left */
  flex-direction: column;        /* ↓ top to bottom */
  flex-direction: column-reverse;/* ↑ bottom to top */

  /* Wrapping */
  flex-wrap: nowrap;   /* all in one line (default) */
  flex-wrap: wrap;     /* wrap to next line if needed */

  /* Shorthand */
  flex-flow: row wrap;  /* direction + wrap */

  /* Justify Content - alignment on MAIN AXIS */
  justify-content: flex-start;    /* [A B C        ] */
  justify-content: flex-end;      /* [        A B C ] */
  justify-content: center;        /* [   A B C      ] */
  justify-content: space-between; /* [A    B    C   ] */
  justify-content: space-around;  /* [ A   B   C  ] */
  justify-content: space-evenly;  /* [  A   B   C  ] */

  /* Align Items - alignment on CROSS AXIS */
  align-items: stretch;      /* fill full height (default) */
  align-items: flex-start;   /* align to top */
  align-items: flex-end;     /* align to bottom */
  align-items: center;       /* center vertically */
  align-items: baseline;     /* align by text baseline */

  /* Align Content - when multiple rows (needs flex-wrap) */
  align-content: flex-start;
  align-content: center;
  align-content: space-between;

  /* Gap between items */
  gap: 20px;           /* same gap all sides */
  gap: 20px 10px;      /* row-gap column-gap */
  row-gap: 20px;
  column-gap: 10px;
}
```

### Item Properties (Children):
```css
.item {
  /* Order */
  order: 0;        /* default: 0 */
  order: -1;       /* move to front */
  order: 1;        /* move to back */

  /* Grow - how much item grows to fill space */
  flex-grow: 0;    /* don't grow (default) */
  flex-grow: 1;    /* grow to fill available space */
  flex-grow: 2;    /* grow twice as much as flex-grow:1 */

  /* Shrink - how much item shrinks when space is tight */
  flex-shrink: 1;  /* can shrink (default) */
  flex-shrink: 0;  /* don't shrink (stays fixed size) */

  /* Basis - initial size before grow/shrink */
  flex-basis: auto;    /* use width/height */
  flex-basis: 200px;   /* start at 200px */
  flex-basis: 0;       /* start at nothing */

  /* Shorthand: flex: grow shrink basis */
  flex: 1;        /* flex: 1 1 0% — most common! */
  flex: auto;     /* flex: 1 1 auto */
  flex: none;     /* flex: 0 0 auto — no grow, no shrink */

  /* Align self — override parent's align-items */
  align-self: center;
  align-self: flex-end;
}
```

---

## 🧩 Common Layout Patterns

### 1. Perfect Center (Most Common Interview Question!):
```css
/* Method 1: Flexbox */
.center {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Method 2: Grid */
.center {
  display: grid;
  place-items: center;
}

/* Method 3: Position + Transform */
.parent { position: relative; }
.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

### 2. Navbar Layout:
```css
.navbar {
  display: flex;
  justify-content: space-between;  /* logo left, links right */
  align-items: center;
  padding: 0 20px;
  height: 60px;
}

.nav-links {
  display: flex;
  gap: 20px;
  list-style: none;
}
```

### 3. Card Grid:
```css
.card-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  flex: 1 1 300px;  /* grow, shrink, min-width 300px */
  /* Cards will be minimum 300px, fill available space */
}
```

### 4. Sticky Footer Layout:
```css
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

main {
  flex: 1;  /* takes all available space, pushes footer down */
}
```

---

## 🔲 CSS Grid — Grid Template Areas & 2D Layout

### What is CSS Grid?
- A **2D layout system** — works in rows AND columns simultaneously
- Perfect for **page layouts, complex grids**
- More powerful than Flexbox for 2D layouts

```css
.container {
  display: grid;
}
```

### Defining Grid:
```css
.container {
  display: grid;

  /* Define columns */
  grid-template-columns: 200px 1fr 200px;     /* fixed | flexible | fixed */
  grid-template-columns: repeat(3, 1fr);       /* 3 equal columns */
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* responsive! */

  /* Define rows */
  grid-template-rows: 80px 1fr 60px;          /* header | main | footer */
  grid-template-rows: repeat(3, 200px);

  /* Gap between cells */
  gap: 20px;
  column-gap: 20px;
  row-gap: 10px;
}
```

### `fr` Unit (Fraction):
```css
/* 3 columns: first gets 1 part, second gets 2 parts, third gets 1 part */
grid-template-columns: 1fr 2fr 1fr;
/* Total: 4 parts — 25% | 50% | 25% */
```

### Grid Template Areas (Most Readable!):
```css
.layout {
  display: grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-rows: 80px 1fr 60px;
  grid-template-areas:
    "header  header  header"   /* row 1 */
    "sidebar main    aside"    /* row 2 */
    "footer  footer  footer";  /* row 3 */
  gap: 10px;
  min-height: 100vh;
}

/* Assign elements to areas */
header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
main    { grid-area: main; }
aside   { grid-area: aside; }
footer  { grid-area: footer; }
```

### Responsive with Grid Areas:
```css
/* Mobile: single column */
@media (max-width: 768px) {
  .layout {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "main"
      "sidebar"
      "aside"
      "footer";
  }
}
```

### Placing Items Manually:
```css
.item {
  /* Column placement */
  grid-column: 1 / 3;        /* from line 1 to line 3 (span 2 cols) */
  grid-column: 1 / -1;       /* from first to last (full width) */
  grid-column: span 2;       /* span 2 columns */

  /* Row placement */
  grid-row: 1 / 3;           /* span 2 rows */
  grid-row: span 2;
}
```

### `minmax()` in Grid:
```css
/* Column is minimum 250px, maximum 1fr */
grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
/* Creates as many columns as fit, each min 250px */
/* Automatically responsive! No media queries needed */
```

### Alignment in Grid:
```css
.container {
  /* Align all items */
  justify-items: start | center | end | stretch;
  align-items:   start | center | end | stretch;
  place-items: center;   /* shorthand: align-items justify-items */

  /* Align the entire grid */
  justify-content: start | center | end | space-between;
  align-content: start | center | end;
}

.item {
  /* Individual item alignment */
  justify-self: center;
  align-self: end;
  place-self: center;
}
```

---

## 🏗️ Combining Grid & Flex — Real-World Layout

```css
/* Grid for overall page structure (2D) */
.page-layout {
  display: grid;
  grid-template-areas:
    "header"
    "main"
    "footer";
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

/* Flex for navbar items (1D) */
header {
  grid-area: header;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 24px;
}

/* Grid for main content area */
main {
  grid-area: main;
  display: grid;
  grid-template-columns: 250px 1fr;
  gap: 24px;
  padding: 24px;
}

/* Flex for card grid inside main */
.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}

.card {
  flex: 1 1 280px;
  /* Each card is internally a flex column */
  display: flex;
  flex-direction: column;
}

.card-footer {
  margin-top: auto;  /* Push footer to bottom of card */
}
```

> **Rule of Thumb:** Use **Grid** for page/section layout, Use **Flex** for component-level layout

---

## 📍 Positioning

### `static` (default)
- Normal document flow
- `top`, `left`, `right`, `bottom` have **no effect**
```css
.element { position: static; }
```

### `relative`
- Element stays in **normal flow**
- Can move using `top`, `left`, `right`, `bottom` **relative to itself**
- Creates a **positioning context** for absolute children
```css
.element {
  position: relative;
  top: 20px;    /* move down 20px from its original position */
  left: 10px;   /* move right 10px */
}
```

### `absolute`
- **Removed from normal flow** (other elements ignore it)
- Positioned relative to **nearest positioned ancestor** (relative/absolute/fixed)
- If no positioned ancestor → positions relative to `<html>`
```css
.parent { position: relative; }  /* Positioning context */

.child {
  position: absolute;
  top: 0;
  right: 0;    /* Top-right corner of parent */
}
```

### `fixed`
- **Removed from normal flow**
- Positioned relative to **viewport** (stays fixed on screen)
- **Doesn't move when scrolling**
```css
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;   /* Stick to top of screen always */
  z-index: 100;
}

.back-to-top {
  position: fixed;
  bottom: 30px;
  right: 30px;   /* Always in bottom-right corner */
}
```

### `sticky`
- Behaves like `relative` until **scroll threshold**
- Then becomes **fixed** (sticks to position)
- Returns to normal flow when parent leaves viewport
```css
.header {
  position: sticky;
  top: 0;    /* Sticks when reaches top of viewport while scrolling */
  z-index: 10;
}

.table-heading {
  position: sticky;
  top: 60px;   /* Sticks 60px from top (below fixed navbar) */
}
```

### Positioning Summary:

| Value | In Flow? | Relative To | Scrolls With Page? |
|-------|----------|-------------|-------------------|
| static | ✅ Yes | Normal flow | ✅ Yes |
| relative | ✅ Yes | Its own position | ✅ Yes |
| absolute | ❌ No | Nearest positioned parent | ✅ Yes |
| fixed | ❌ No | Viewport | ❌ No (stays) |
| sticky | ✅/❌ | Scroll position | Until threshold |

---

## 📚 Stacking Context — Understanding `z-index`

### What is z-index?
- Controls the **stack order** of elements (which appears on top)
- Higher `z-index` = appears in front
- Works only on **positioned elements** (not static)

```css
.element {
  position: relative;  /* Must have position! */
  z-index: 10;
}
```

### Stacking Order (Default, no z-index):
```
1. Background and borders of root element
2. Non-positioned block elements (in DOM order)
3. Floating elements
4. Non-positioned inline elements
5. Positioned elements (z-index: auto / 0) in DOM order
6. Positioned elements with z-index > 0
```

### What Creates a New Stacking Context:
```css
/* These create NEW stacking context */
position: relative/absolute/fixed/sticky + z-index (not auto)
opacity: less than 1
transform: any value
filter: any value
isolation: isolate
```

### Common z-index Scale:
```css
:root {
  --z-below:   -1;
  --z-normal:   0;
  --z-raised:  10;
  --z-dropdown: 100;
  --z-sticky:  200;
  --z-overlay: 300;
  --z-modal:   400;
  --z-toast:   500;
}
```

### Common z-index Trap:
```css
/* ❌ This won't work as expected! */
.parent {
  position: relative;
  z-index: 1;           /* Creates stacking context */
}
.child {
  position: absolute;
  z-index: 9999;        /* Trapped inside parent's context */
  /* Child can NEVER appear above elements outside parent */
}
```

---

## 📦 Container Queries — Modern Responsive Component Design

### Problem with Media Queries:
- Media queries respond to **viewport size**
- But a component might be in a **narrow sidebar or wide main area**
- Same viewport, different component sizes → media queries can't handle this

### Container Queries — Solution!
- Styles based on **container's size** (not viewport)
- Component responds to its **own container**

```css
/* Step 1: Define container */
.card-wrapper {
  container-type: inline-size;  /* Enable size queries */
  container-name: card;         /* Optional: name it */
}

/* Step 2: Query the container */
@container (min-width: 400px) {
  .card {
    display: flex;
    flex-direction: row;
  }
}

@container (max-width: 399px) {
  .card {
    display: flex;
    flex-direction: column;
  }
}
```

### Real-World Example:
```css
/* Same card component, different layouts based on container */
.card-container {
  container-type: inline-size;
}

/* Small container (sidebar) — stack vertically */
.card {
  display: flex;
  flex-direction: column;
}

/* Large container (main content) — lay horizontal */
@container (min-width: 500px) {
  .card {
    flex-direction: row;
  }

  .card-image {
    width: 200px;
    flex-shrink: 0;
  }
}
```

### Container Queries vs Media Queries:

| Feature | Media Query | Container Query |
|---------|-------------|----------------|
| Based on | Viewport width | Container width |
| Reusable components | ❌ Hard | ✅ Easy |
| Sidebar vs Main | ❌ Can't distinguish | ✅ Responds correctly |
| Browser support | ✅ All browsers | ✅ Modern browsers |

---

## 🎯 Quick Revision Summary

| Topic | Key Point |
|-------|-----------|
| Browser Rendering | HTML→DOM, CSS→CSSOM, DOM+CSSOM→Render Tree→Paint |
| Reflow | Layout change — expensive, avoid unnecessary |
| Repaint | Visual change only — less expensive |
| Semantic HTML | Meaningful tags for SEO + Accessibility |
| ARIA | Extra attributes for screen readers |
| CSS Specificity | Inline(1000) > ID(100) > Class(10) > Element(1) |
| Box Model | Content → Padding → Border → Margin |
| `border-box` | Width includes padding + border (use always!) |
| Flexbox | 1D layout, main axis + cross axis |
| CSS Grid | 2D layout, rows + columns |
| `clamp()` | Min/preferred/max for fluid design |
| CSS Variables | `--name: value` defined in `:root` |
| SASS | CSS preprocessor with variables, nesting, mixins |
| `z-index` | Stack order, only works on positioned elements |
| Container Queries | Respond to container size, not viewport |

---