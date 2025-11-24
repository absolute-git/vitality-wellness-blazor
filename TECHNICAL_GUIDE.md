# Vitality Wellness Center - Technical Guide

A comprehensive guide to understanding the architecture, theming system, and styling of this Blazor WebAssembly website.

---

## Table of Contents

1. [Project Architecture](#project-architecture)
2. [Blazor Component Structure](#blazor-component-structure)
3. [CSS Theming System](#css-theming-system)
4. [Color Palette](#color-palette)
5. [Typography](#typography)
6. [Layout System](#layout-system)
7. [Component Styling](#component-styling)
8. [Responsive Design](#responsive-design)
9. [Customization Guide](#customization-guide)

---

## Project Architecture

### Technology Stack

| Technology | Purpose |
|------------|---------|
| .NET 8 | Runtime framework |
| Blazor WebAssembly | Client-side web framework |
| C# | Programming language for components |
| Razor | Template syntax for HTML generation |
| CSS3 | Styling with custom properties (variables) |
| Bootstrap | Grid system (minimal usage) |

### How Blazor WebAssembly Works

```
┌─────────────────────────────────────────────────────────────┐
│                        Browser                               │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐     │
│  │  index.html │───>│ blazor.js   │───>│ .NET WASM   │     │
│  │  (entry)    │    │ (loader)    │    │ (runtime)   │     │
│  └─────────────┘    └─────────────┘    └──────┬──────┘     │
│                                                │            │
│                                         ┌──────▼──────┐     │
│                                         │   App.razor │     │
│                                         │  (router)   │     │
│                                         └──────┬──────┘     │
│                                                │            │
│         ┌──────────────────────────────────────┼───────┐    │
│         │                                      │       │    │
│  ┌──────▼──────┐  ┌──────────┐  ┌──────────┐  │       │    │
│  │ MainLayout  │  │  About   │  │ Services │  │ ...   │    │
│  │  (shell)    │  │  .razor  │  │  .razor  │  │       │    │
│  └─────────────┘  └──────────┘  └──────────┘  └───────┘    │
└─────────────────────────────────────────────────────────────┘
```

### File Structure Explained

```
MyWebSite/
│
├── wwwroot/                    # Static assets (served directly)
│   ├── index.html              # HTML entry point
│   ├── css/
│   │   ├── app.css             # Main stylesheet (all custom styles)
│   │   └── bootstrap/          # Bootstrap CSS (grid only)
│   ├── favicon.png             # Browser tab icon
│   └── sample-data/            # Static JSON data
│
├── Layout/                     # Shared layout components
│   ├── MainLayout.razor        # Main page wrapper (header + footer)
│   └── NavMenu.razor           # Navigation bar component
│
├── Pages/                      # Routable page components
│   ├── Home.razor              # "/" route
│   ├── About.razor             # "/about" route
│   ├── Services.razor          # "/services" route
│   └── Contact.razor           # "/contact" route
│
├── App.razor                   # Root component with router
├── _Imports.razor              # Global using statements
├── Program.cs                  # Application entry point
└── MyWebSite.csproj            # Project configuration
```

---

## Blazor Component Structure

### Anatomy of a Razor Component

Each `.razor` file is a self-contained component with three sections:

```razor
@page "/route"                           ← 1. DIRECTIVES (routing, using, etc.)

<PageTitle>Page Title</PageTitle>        ← 2. HTML MARKUP (with Razor syntax)

<section class="my-section">
    <h1>@title</h1>                       ← Data binding with @

    @if (showContent)                     ← Conditional rendering
    {
        <p>@message</p>
    }

    @foreach (var item in items)          ← Loop rendering
    {
        <div>@item.Name</div>
    }
</section>

@code {                                   ← 3. C# CODE BLOCK
    private string title = "Hello";
    private bool showContent = true;
    private List<Item> items = new();
}
```

### Component Hierarchy

```
App.razor
└── Router
    └── MainLayout.razor
        ├── NavMenu.razor (header)
        ├── @Body (page content)
        │   ├── Home.razor
        │   ├── About.razor
        │   ├── Services.razor
        │   └── Contact.razor
        └── Footer (in MainLayout)
```

### Navigation System

The `NavLink` component provides automatic active state styling:

```razor
<NavLink class="nav-link" href="about">About</NavLink>
```

When the URL matches `/about`, Blazor automatically adds the `active` class.

---

## CSS Theming System

### CSS Custom Properties (Variables)

The entire theme is controlled through CSS variables defined in `:root`. This allows easy customization without searching through the stylesheet.

```css
:root {
    /* ===== Primary Brand Colors ===== */
    --primary-color: #10b981;      /* Main green - buttons, links, accents */
    --primary-dark: #059669;       /* Darker green - hover states */
    --primary-light: #34d399;      /* Lighter green - highlights */

    /* ===== Secondary Colors ===== */
    --secondary-color: #1e3a5f;    /* Dark blue - headings, footer */
    --accent-color: #0891b2;       /* Cyan - gradients, special elements */
    --warm-accent: #f59e0b;        /* Amber - optional warm accent */

    /* ===== Text Colors ===== */
    --text-color: #1e293b;         /* Primary text - dark slate */
    --text-light: #64748b;         /* Secondary text - muted */
    --text-white: #ffffff;         /* White text on dark backgrounds */

    /* ===== Background Colors ===== */
    --bg-color: #ffffff;           /* Main background */
    --bg-light: #f0fdf4;           /* Light green tint sections */
    --bg-warm: #fffbeb;            /* Warm background option */
    --bg-dark: #1e3a5f;            /* Dark sections (hero, footer) */

    /* ===== Utility ===== */
    --border-color: #e2e8f0;       /* Borders and dividers */

    /* ===== Shadows ===== */
    --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
    --shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
    --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1);
    --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1);

    /* ===== Border Radius ===== */
    --radius: 8px;                 /* Standard corners */
    --radius-lg: 16px;             /* Cards, images */
    --radius-xl: 24px;             /* Hero images */

    /* ===== Animation ===== */
    --transition: all 0.3s ease;   /* Smooth transitions */
}
```

### Using Variables in CSS

Variables are referenced with `var()`:

```css
.button {
    background: var(--primary-color);
    border-radius: var(--radius);
    transition: var(--transition);
}

.button:hover {
    background: var(--primary-dark);
    box-shadow: var(--shadow-lg);
}
```

### Theme Switching (Advanced)

To create a dark mode, you could add:

```css
[data-theme="dark"] {
    --bg-color: #0f172a;
    --text-color: #e2e8f0;
    --border-color: #334155;
}
```

Then toggle with JavaScript: `document.documentElement.dataset.theme = 'dark'`

---

## Color Palette

### Visual Color Reference

```
PRIMARY (Green - Wellness/Health)
┌─────────────────────────────────────────────────┐
│  #34d399       #10b981       #059669           │
│  Light         Primary       Dark              │
│  ████████      ████████      ████████          │
└─────────────────────────────────────────────────┘

SECONDARY (Blue - Trust/Professionalism)
┌─────────────────────────────────────────────────┐
│  #0891b2       #1e3a5f                         │
│  Accent        Secondary                        │
│  ████████      ████████                         │
└─────────────────────────────────────────────────┘

NEUTRALS (Text & Backgrounds)
┌─────────────────────────────────────────────────┐
│  #ffffff  #f0fdf4  #e2e8f0  #64748b  #1e293b   │
│  White    Light    Border   Muted    Text      │
│  ████████ ████████ ████████ ████████ ████████  │
└─────────────────────────────────────────────────┘
```

### Color Usage Guidelines

| Color | Usage |
|-------|-------|
| `--primary-color` | Buttons, links, icons, accents |
| `--primary-dark` | Hover states, active states |
| `--primary-light` | Badges, highlights, stats |
| `--secondary-color` | Headings, footer background |
| `--accent-color` | Gradients, special highlights |
| `--text-color` | Body text, paragraphs |
| `--text-light` | Captions, secondary info |
| `--bg-light` | Alternating section backgrounds |

---

## Typography

### Font Stack

```css
body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont,
                 'Segoe UI', Roboto, sans-serif;
    font-size: 16px;
    line-height: 1.6;
}
```

The Inter font is loaded from Google Fonts in `index.html`:

```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

### Type Scale

```css
h1 { font-size: 2.5rem; }    /* 40px - Page titles */
h2 { font-size: 2rem; }      /* 32px - Section titles */
h3 { font-size: 1.25rem; }   /* 20px - Card titles */
body { font-size: 1rem; }    /* 16px - Body text */
small { font-size: 0.875rem; } /* 14px - Captions */
```

### Font Weights Used

| Weight | Usage |
|--------|-------|
| 300 | Light accents |
| 400 | Body text |
| 500 | Navigation, labels |
| 600 | Headings, buttons |
| 700 | Hero titles, brand |

---

## Layout System

### Container

The `.container` class constrains content width and centers it:

```css
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1.5rem;
}
```

### CSS Grid Usage

Most layouts use CSS Grid for responsive design:

```css
/* Two-column layout */
.hero-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: center;
}

/* Auto-fit responsive grid */
.features-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
}
```

### Flexbox Usage

Flexbox is used for inline layouts:

```css
.navbar .container {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.hero-actions {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
}
```

### Section Spacing

```css
section {
    padding: 5rem 0;    /* Vertical spacing */
}

.page-header {
    padding: 4rem 0;    /* Slightly less for headers */
}
```

---

## Component Styling

### Buttons

```css
.btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
    font-weight: 500;
    border-radius: var(--radius);
    border: 2px solid transparent;
    cursor: pointer;
    transition: var(--transition);
    text-decoration: none;
}

/* Primary - filled green */
.btn-primary {
    background: var(--primary-color);
    color: var(--text-white);
    border-color: var(--primary-color);
}

.btn-primary:hover {
    background: var(--primary-dark);
    border-color: var(--primary-dark);
}

/* Outline - transparent with border */
.btn-outline {
    background: transparent;
    color: var(--primary-color);
    border-color: var(--primary-color);
}

.btn-outline:hover {
    background: var(--primary-color);
    color: var(--text-white);
}
```

### Cards

```css
.feature-card {
    background: var(--bg-color);
    padding: 2rem;
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow);
    text-align: center;
    transition: var(--transition);
}

.feature-card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-lg);
}
```

### Icons

Icons are inline SVGs with stroke styling:

```html
<div class="feature-icon">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M22 12h-4l-3 9L9 3l-3 9H2"/>
    </svg>
</div>
```

```css
.feature-icon {
    width: 60px;
    height: 60px;
    background: linear-gradient(135deg, var(--primary-color), var(--accent-color));
    border-radius: var(--radius-lg);
    display: flex;
    align-items: center;
    justify-content: center;
}

.feature-icon svg {
    width: 30px;
    height: 30px;
    stroke: white;
}
```

### Form Controls

```css
.form-control {
    padding: 0.75rem 1rem;
    border: 1px solid var(--border-color);
    border-radius: var(--radius);
    font-size: 1rem;
    font-family: inherit;
    transition: var(--transition);
}

.form-control:focus {
    outline: none;
    border-color: var(--primary-color);
    box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.1);
}
```

---

## Responsive Design

### Breakpoint Strategy

The site uses a single main breakpoint at 768px (tablet):

```css
@media (max-width: 768px) {
    /* Mobile styles */
}
```

### Mobile Adaptations

```css
@media (max-width: 768px) {
    /* Typography scales down */
    h1 { font-size: 2rem; }
    h2 { font-size: 1.5rem; }

    /* Grid becomes single column */
    .hero-grid,
    .about-grid,
    .contact-grid {
        grid-template-columns: 1fr;
    }

    /* Navigation becomes hamburger menu */
    .navbar-toggle {
        display: block;
    }

    .navbar-nav {
        position: absolute;
        top: 70px;
        left: 0;
        right: 0;
        flex-direction: column;
    }

    .navbar-nav.collapsed {
        display: none;
    }

    /* Footer stacks vertically */
    .footer-content {
        grid-template-columns: 1fr;
        text-align: center;
    }
}
```

### Mobile Navigation Toggle

The hamburger menu is controlled by Blazor:

```razor
<!-- NavMenu.razor -->
<button class="navbar-toggle" @onclick="ToggleNavMenu">
    <span class="toggle-icon @(collapseNavMenu ? "" : "open")"></span>
</button>

<nav class="navbar-nav @NavMenuCssClass">
    <!-- nav links -->
</nav>

@code {
    private bool collapseNavMenu = true;
    private string? NavMenuCssClass => collapseNavMenu ? "collapsed" : null;

    private void ToggleNavMenu()
    {
        collapseNavMenu = !collapseNavMenu;
    }
}
```

---

## Customization Guide

### Changing the Color Scheme

To change the entire theme, modify the CSS variables in `wwwroot/css/app.css`:

**Example: Blue theme**
```css
:root {
    --primary-color: #3b82f6;
    --primary-dark: #2563eb;
    --primary-light: #60a5fa;
    --bg-light: #eff6ff;
}
```

**Example: Purple theme**
```css
:root {
    --primary-color: #8b5cf6;
    --primary-dark: #7c3aed;
    --primary-light: #a78bfa;
    --bg-light: #f5f3ff;
}
```

### Changing the Font

1. Update the Google Fonts link in `wwwroot/index.html`:
```html
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
```

2. Update the font-family in `app.css`:
```css
body {
    font-family: 'Poppins', sans-serif;
}
```

### Adding New Pages

1. Create a new file in `Pages/`:
```razor
@page "/new-page"

<PageTitle>New Page - Vitality</PageTitle>

<section class="page-header">
    <div class="container">
        <h1>New Page</h1>
        <p>Description here</p>
    </div>
</section>

<section class="content">
    <div class="container">
        <!-- Your content -->
    </div>
</section>
```

2. Add a link in `NavMenu.razor`:
```razor
<NavLink class="nav-link" href="new-page">New Page</NavLink>
```

### Replacing Images

Images are loaded from Unsplash via URL. To use local images:

1. Add images to `wwwroot/images/`
2. Reference them in Razor:
```html
<img src="images/my-image.jpg" alt="Description" />
```

### Adding New Sections

Use the existing section pattern:

```html
<section class="new-section">
    <div class="container">
        <h2 class="section-title">Section Title</h2>
        <p class="section-subtitle">Optional subtitle</p>

        <div class="new-section-grid">
            <!-- Content -->
        </div>
    </div>
</section>
```

```css
.new-section {
    padding: 5rem 0;
    background: var(--bg-light); /* or var(--bg-color) */
}

.new-section-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
}
```

---

## Key CSS Patterns Reference

### Gradient Backgrounds
```css
background: linear-gradient(135deg, var(--bg-dark) 0%, #1e3a5f 100%);
```

### Hover Lift Effect
```css
transition: var(--transition);

&:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-lg);
}
```

### Focus Ring
```css
&:focus {
    outline: none;
    box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.1);
}
```

### Truncate Text
```css
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```

### Aspect Ratio (Images)
```css
.image-container img {
    width: 100%;
    height: 200px;
    object-fit: cover;
}
```

---

## Performance Tips

1. **Images**: Use Unsplash's URL parameters for optimized sizes:
   ```
   ?w=600&h=400&fit=crop
   ```

2. **CSS**: All styles are in one file to reduce HTTP requests

3. **Fonts**: Only load needed weights (300-700)

4. **Blazor**: Published output is AOT-compiled and trimmed

---

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

CSS features used (all well-supported):
- CSS Grid
- CSS Flexbox
- CSS Custom Properties
- CSS Transitions

---

*Last updated: November 2024*
