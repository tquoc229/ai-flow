# Design Guideline - Anphabe Research Insights

**Project:** Anphabe Research Insights
**Version:** 1.0
**Last Updated:** January 2025
**Tech Stack:** Next.js 15, TypeScript, Tailwind CSS, Directus CMS

---

## Table of Contents

1. [Overall Design Principles](#overall-design-principles)
2. [Color System](#color-system)
3. [Typography](#typography)
4. [Component Library](#component-library)
5. [Layout & Grid System](#layout--grid-system)
6. [Icons & Images](#icons--images)
7. [Spacing & Sizing](#spacing--sizing)
8. [Interactive States](#interactive-states)
9. [Responsive Design](#responsive-design)
10. [Code Examples](#code-examples)

---

## Overall Design Principles

### Design Philosophy

The Anphabe Research Insights platform embodies a **professional, content-first design** approach that prioritizes:

- **Clarity & Readability**: Clean typography and ample whitespace for optimal reading experience
- **Trust & Authority**: Professional color palette and structured layouts that convey expertise
- **Accessibility**: WCAG-compliant color contrast and semantic HTML
- **Performance**: Optimized images and efficient component rendering
- **Consistency**: Unified design tokens and reusable component patterns

### Visual Identity

- **Professional & Trustworthy**: Enterprise-grade HR research platform
- **Clean & Modern**: Minimalist aesthetic with purposeful use of color
- **Content-Focused**: Design serves the content, not overshadowing it
- **Vietnamese Market**: Culturally appropriate design choices for Vietnam audience

---

## Color System

### Design Token Architecture

The project uses **HSL-based CSS custom properties** for flexible theming with light/dark mode support.

**Location:** `app/globals.css` (lines 24-69)
**Configuration:** `tailwind.config.ts` (lines 37-78)

### Primary Colors

```css
/* Light Mode (Root) */
--primary: 0 0% 9%;           /* Almost black - #171717 */
--primary-foreground: 0 0% 98%; /* Off-white text */

/* Dark Mode */
--primary: 0 0% 98%;          /* Off-white - #FAFAFA */
--primary-foreground: 0 0% 9%; /* Dark text */
```

**Usage:**
- Primary actions and emphasis
- Main headings and important UI elements
- Use sparingly for maximum impact

### Brand Colors

```css
/* Anphabe Brand Orange */
#fa770c  /* Primary CTA color */
#e56c0b  /* Hover state - 10% darker */
#e56a0a  /* Alternative hover variant */

/* Anphabe Brand Teal */
#087096  /* Secondary brand color */
/* Used for: Links, secondary actions, category badges */
```

**Implementation Example:**
```tsx
// Header.tsx:214
<Link
  href="/contact"
  className="bg-[#fa770c] text-white px-4 py-2 rounded-md
             font-medium hover:bg-[#e56c0b] transition-colors"
>
  LIÊN HỆ
</Link>
```

### Semantic Colors

#### Background Colors
```css
--background: 0 0% 100%;      /* White - #FFFFFF */
--card: 0 0% 100%;            /* Card background */
--popover: 0 0% 100%;         /* Dropdown/modal background */

/* Dark Mode */
--background: 0 0% 3.9%;      /* Near black - #0A0A0A */
--card: 0 0% 3.9%;
```

#### Text Colors
```css
--foreground: 0 0% 3.9%;      /* Primary text - #0A0A0A */
--muted-foreground: 0 0% 45.1%; /* Secondary text - #6B7280 */
```

#### Border & Input Colors
```css
--border: 0 0% 89.8%;         /* Light gray - #E5E5E5 */
--input: 0 0% 89.8%;          /* Input borders */
--ring: 0 0% 3.9%;            /* Focus ring color */

/* Dark Mode */
--border: 0 0% 14.9%;         /* Dark gray - #262626 */
--input: 0 0% 14.9%;
--ring: 0 0% 83.1%;
```

#### State Colors
```css
--destructive: 0 84.2% 60.2%;  /* Red for errors - #EF4444 */
--destructive-foreground: 0 0% 98%;
--accent: 0 0% 96.1%;          /* Subtle highlight - #F5F5F5 */
--accent-foreground: 0 0% 9%;
--secondary: 0 0% 96.1%;       /* Secondary actions */
--secondary-foreground: 0 0% 9%;
```

### Chart Colors

```css
--chart-1: hsl(var(--chart-1));
--chart-2: hsl(var(--chart-2));
--chart-3: hsl(var(--chart-3));
--chart-4: hsl(var(--chart-4));
--chart-5: hsl(var(--chart-5));
```

### Gray Scale

From `tailwind.config.ts` and usage patterns:

```
gray-50:   #FAFAFA  (Backgrounds)
gray-100:  #F5F5F5  (Subtle backgrounds)
gray-200:  #E5E5E5  (Borders, dividers)
gray-300:  #D4D4D4  (Disabled states)
gray-400:  #A3A3A3  (Placeholder text)
gray-500:  #737373  (Secondary text)
gray-600:  #525252  (Body text)
gray-700:  #404040  (Dark text)
gray-800:  #262626  (Headings, footer)
gray-900:  #171717  (Copyright, legal)
```

### Content-Specific Colors

#### Blockquotes
```css
/* globals.css:86 */
.prose blockquote {
  border-left: 4px solid #fa770c; /* Anphabe orange accent */
  color: #555555;
}
```

#### Code Blocks
```css
/* Light Mode */
background: #FAFAFA (gray-50)
border: #E5E5E5 (gray-200)
text: #262626 (gray-800)

/* Dark Mode */
background: #171717 (gray-900)
border: #404040 (gray-700)
text: #E5E5E5 (gray-200)
```

### Color Usage Guidelines

#### Primary Actions
- **Use:** `#fa770c` (orange) for all primary CTAs
- **Hover:** `#e56c0b` (darker orange)
- **Examples:** Download buttons, form submissions, main navigation CTA

#### Links & Navigation
- **Default:** `text-gray-700` for navigation items
- **Hover:** `text-[#087096]` (teal) or `text-[#fa770c]` (orange)
- **Active:** Maintain hover color + font-weight change

#### Backgrounds
- **Page Background:** `bg-white` or `bg-gray-50`
- **Card Backgrounds:** `bg-white` with shadow
- **Footer:** `bg-gray-800` (dark) and `bg-gray-900` (darker)

#### Borders
- **Light:** `border-gray-200` for subtle divisions
- **Medium:** `border-gray-300` for form inputs
- **Hover:** No border color change, use shadow instead

---

## Typography

### Font Family

**Primary Font:** **Inter** (Google Fonts)
**Display Font:** **Oswald** (Bold, 700 weight) for special headings

**Configuration:** `app/layout.tsx`
```tsx
import { Inter } from "next/font/google";

const inter = Inter({ subsets: ["latin"] });
```

```html
<!-- Oswald for display/impact -->
<link href="https://fonts.googleapis.com/css2?family=Oswald:wght@700&display=swap"
      rel="stylesheet" />
```

### Font Size Scale

**Location:** `tailwind.config.ts` (lines 22-36)

```typescript
fontSize: {
  xs: ["0.75rem", { lineHeight: "1rem" }],      // 12px / 16px
  sm: ["0.875rem", { lineHeight: "1.25rem" }],  // 14px / 20px
  base: ["1rem", { lineHeight: "1.5rem" }],     // 16px / 24px
  lg: ["1.125rem", { lineHeight: "1.75rem" }],  // 18px / 28px
  xl: ["1.25rem", { lineHeight: "1.75rem" }],   // 20px / 28px
  "2xl": ["1.5rem", { lineHeight: "2rem" }],    // 24px / 32px
  "3xl": ["1.875rem", { lineHeight: "2.25rem" }], // 30px / 36px
  "4xl": ["2.25rem", { lineHeight: "2.5rem" }],   // 36px / 40px
  "5xl": ["3rem", { lineHeight: "1.2" }],         // 48px / 57.6px
  "6xl": ["3.75rem", { lineHeight: "1.2" }],      // 60px / 72px
  "7xl": ["4.5rem", { lineHeight: "1.2" }],       // 72px / 86.4px
  "8xl": ["6rem", { lineHeight: "1.2" }],         // 96px / 115.2px
  "9xl": ["8rem", { lineHeight: "1.2" }],         // 128px / 153.6px
}
```

### Typography Patterns

#### Page Titles (H1)
```tsx
// Example: HeroFeaturedSection.tsx:61
<h1 className="text-3xl md:text-4xl font-bold font-mulish
                text-[#087096] text-center">
  {title}
</h1>
```
- **Mobile:** `text-3xl` (30px)
- **Desktop:** `text-4xl` (36px)
- **Weight:** `font-bold` (700)
- **Color:** Brand teal (`#087096`)

#### Article Titles (H2)
```tsx
// Example: PostCard.tsx:81-83
<h3 className="text-lg font-semibold line-clamp-2
                hover:text-[#fa770c] transition-colors mb-2">
  {post.title}
</h3>
```
- **Size:** `text-lg` (18px) for cards
- **Size:** `text-xl sm:text-2xl md:text-3xl` for hero articles
- **Weight:** `font-semibold` (600) or `font-bold` (700)
- **Hover:** Orange color change

#### Body Text
```tsx
// Example: PostCard.tsx:86-90
<p className="text-gray-600 text-sm mb-3 line-clamp-3">
  {post.excerpt}
</p>
```
- **Size:** `text-sm` (14px) for excerpts
- **Size:** `text-base` (16px) for article body
- **Color:** `text-gray-600` for secondary text
- **Line Clamp:** Use `line-clamp-{n}` for truncation

#### Category Labels
```tsx
// Example: PostCard.tsx:71
<Link className="text-xs font-medium text-gray-500
                 hover:text-[#fa770c] transition-colors">
  {category}
</Link>
```
- **Size:** `text-xs` (12px)
- **Weight:** `font-medium` (500)
- **Color:** `text-gray-500`

#### Navigation Text
```tsx
// Example: Header.tsx:56
<Link className="text-gray-700 hover:text-[#087096] font-medium">
  Trang chủ
</Link>
```
- **Size:** Default (`text-base`)
- **Weight:** `font-medium` (500)
- **Color:** `text-gray-700` → hover: `text-[#087096]`

#### Form Labels & Inputs
```tsx
// Example: TextInput.tsx:26-36
<input className="text-gray-800 leading-tight py-3 px-4" />
```
- **Input Text:** `text-gray-800`
- **Placeholder:** Lighter gray (browser default)
- **Error Messages:** `text-red-500 text-xs italic`

### Line Height Guidelines

- **Tight (1.2):** Display text, large headings (5xl+)
- **Normal (1.5):** Body text, paragraphs (base, sm)
- **Relaxed (1.75):** Links, navigation (lg, xl)
- **Loose (2.0):** Large titles (2xl)

### Font Weight Usage

```
font-normal   400  - Body text, regular content
font-medium   500  - Navigation, labels, category tags
font-semibold 600  - Card titles, subheadings
font-bold     700  - Page titles, CTAs, emphasis
```

### Prose Styling (Article Content)

**Location:** `app/globals.css` (lines 80-172)

```css
.prose img {
  @apply mx-auto rounded-lg;
}

.prose blockquote {
  border-left: 4px solid #fa770c;
  padding-left: 1rem;
  font-style: italic;
  color: #555;
}

.prose code {
  @apply text-gray-800 dark:text-gray-200
         px-1 py-0.5 border border-gray-100
         rounded-lg bg-gray-100;
}

.prose h1, .prose h2, .prose h3 {
  margin-top: 1.5rem;
  margin-bottom: 1.5rem;
}
```

### Vietnamese Language Considerations

- **Font Rendering:** System fonts ensure proper Vietnamese diacritical marks
- **Character Spacing:** Default letter-spacing works well for Vietnamese
- **Text Alignment:** Left-aligned for optimal readability

---

## Component Library

### Buttons

#### Primary CTA Button

**Implementation:** `Header.tsx:214`, `RegistrationForm.tsx:478`

```tsx
<button className="bg-[#fa770c] text-white px-6 py-3
                   rounded-lg font-bold hover:bg-[#e56c0b]
                   transition-colors duration-300
                   focus:outline-none focus:ring-4 focus:ring-orange-300
                   disabled:bg-orange-300 disabled:cursor-not-allowed">
  TẢI NGAY
</button>
```

**Properties:**
- **Background:** `bg-[#fa770c]` (Anphabe orange)
- **Hover:** `hover:bg-[#e56c0b]` (darker)
- **Padding:** `px-6 py-3` (24px horizontal, 12px vertical)
- **Border Radius:** `rounded-lg` (8px)
- **Font:** `font-bold text-white`
- **Focus Ring:** `focus:ring-4 focus:ring-orange-300`
- **Disabled State:** `disabled:bg-orange-300 disabled:cursor-not-allowed`

#### Secondary Button (Outlined)

**Implementation:** `Pagination.tsx:66-70`

```tsx
<Link className="px-4 py-2 rounded
                 bg-[#087096] text-white
                 hover:bg-gray-200 text-[#087096]">
  {page}
</Link>
```

**Properties:**
- **Background:** Transparent or light gray
- **Border:** `border border-[#087096]`
- **Text Color:** `text-[#087096]`
- **Hover:** `hover:bg-gray-200`

#### Text Button (Link-style)

```tsx
<button className="text-[#087096] hover:text-[#fa770c]
                   font-medium transition-colors">
  Learn More
</button>
```

#### Button Sizes

```tsx
// Large (Hero CTA)
className="px-8 py-3 text-base md:text-lg"

// Medium (Default)
className="px-6 py-3 text-base"

// Small (Inline actions)
className="px-4 py-2 text-sm"
```

### Form Components

#### Text Input

**Location:** `components/ui/TextInput.tsx`

```tsx
<input
  type="text"
  className="shadow-sm appearance-none border rounded-lg
             w-full py-3 px-4 text-gray-800 leading-tight
             focus:outline-none focus:ring-2 focus:ring-blue-500
             transition duration-200
             border-gray-300" // Error: border-red-500
/>
{error && (
  <p className="text-red-500 text-xs italic mt-1">{error}</p>
)}
```

**Properties:**
- **Border:** `border border-gray-300`
- **Focus:** `focus:ring-2 focus:ring-blue-500`
- **Padding:** `py-3 px-4` (12px vertical, 16px horizontal)
- **Error State:** Red border + error message below

#### Select Dropdown

**Location:** `components/ui/SelectInput.tsx`

```tsx
<select className="shadow-sm appearance-none border rounded-lg
                   w-full py-3 px-4 text-gray-800
                   focus:outline-none focus:ring-2 focus:ring-blue-500
                   border-gray-300">
  <option value="">-- Chọn --</option>
</select>
```

**Properties:** Same as text input
**Note:** Custom arrow using CSS `appearance-none`

#### Form Layout

```tsx
// Full-width inputs
<div className="space-y-5">
  <TextInput />
  <TextInput />
</div>

// Two-column grid
<div className="grid grid-cols-1 sm:grid-cols-2 gap-5">
  <TextInput />
  <TextInput />
</div>
```

### Cards

#### Article Card (PostCard)

**Location:** `components/PostCard.tsx`

```tsx
<article className="group bg-white rounded-lg overflow-hidden
                    shadow hover:shadow-md transition-shadow
                    h-full flex flex-col">
  {/* Image */}
  <div className="relative h-[250px] overflow-hidden">
    <Image className="object-cover hover:scale-105
                      transition-transform duration-500" />
  </div>

  {/* Content */}
  <div className="p-3 md:p-4 flex flex-col flex-grow">
    <Link className="text-xs font-medium text-gray-500
                     hover:text-[#fa770c]">
      Category
    </Link>
    <h3 className="text-lg font-semibold line-clamp-2
                   hover:text-[#fa770c]">
      Title
    </h3>
    <p className="text-gray-600 text-sm line-clamp-3">
      Excerpt
    </p>
  </div>
</article>
```

**Properties:**
- **Background:** `bg-white`
- **Shadow:** `shadow` → `hover:shadow-md`
- **Image Height:** `h-[250px]`
- **Image Hover:** Scale to 105%
- **Padding:** `p-3 md:p-4` (responsive)
- **Layout:** Flexbox column with `flex-grow`

#### Author Card

**Location:** `components/goc-nhin/AuthorCard.tsx`

Similar to PostCard but with:
- **Image Height:** `h-64 sm:h-56 md:h-64`
- **No category label**
- **Title field** instead of excerpt

#### Hero Featured Card

**Location:** `components/sections/HeroFeaturedSection.tsx:68-118`

```tsx
<div className="grid grid-cols-1 lg:grid-cols-12 gap-4 md:gap-8
                bg-white rounded-lg shadow-md overflow-hidden">
  {/* Left: Image (6 cols) */}
  <div className="lg:col-span-6 relative h-[450px]">
    <Image fill className="object-cover" />
  </div>

  {/* Right: Content (6 cols) */}
  <div className="lg:col-span-6 p-4 md:p-6 flex flex-col justify-center">
    <h2 className="text-xl sm:text-2xl md:text-3xl font-bold">Title</h2>
    <p className="text-base md:text-lg line-clamp-3">Excerpt</p>
  </div>
</div>
```

**Properties:**
- **Layout:** 12-column grid (50/50 split on desktop)
- **Image Height:** `h-[450px]`
- **Content Padding:** `p-4 md:p-6`
- **Content Alignment:** `flex flex-col justify-center`

### Navigation

#### Header

**Location:** `components/Header.tsx`

**Desktop Navigation:**
```tsx
<nav className="hidden md:flex items-center space-x-6">
  <Link className="text-gray-700 hover:text-[#087096] font-medium">
    Link
  </Link>

  {/* Dropdown Menu */}
  <div className="relative group">
    <button className="text-gray-700 hover:text-[#087096]
                       font-medium flex items-center">
      Menu
      <ChevronDownIcon />
    </button>
    <div className="absolute left-0 mt-2 w-64 bg-white
                    shadow-lg rounded-md border border-gray-200">
      {/* Dropdown Items */}
    </div>
  </div>
</nav>
```

**Mobile Navigation:**
```tsx
<nav className="md:hidden pt-4 pb-2">
  <div className="flex flex-col space-y-1">
    <Link className="py-2 border-b border-gray-100">Link</Link>
  </div>
</nav>
```

**Properties:**
- **Background:** `bg-white`
- **Border:** `border-b border-gray-200`
- **Shadow:** `shadow-sm`
- **Position:** `sticky top-0 z-50`
- **Container:** `container mx-auto`

#### Footer

**Location:** `components/Footer.tsx`

```tsx
<footer>
  {/* CTA Section */}
  <div className="bg-cover bg-center text-white
                  bg-gray-900 bg-opacity-75"
       style={{backgroundImage: 'url(...)'}}>
    <Link className="bg-orange-500 hover:bg-orange-600
                     text-white font-medium py-2 px-6 rounded">
      CTA
    </Link>
  </div>

  {/* Main Footer */}
  <div className="bg-gray-800 text-white py-10">
    <div className="grid grid-cols-1 md:grid-cols-4 gap-8">
      {/* Footer Columns */}
    </div>
  </div>

  {/* Copyright */}
  <div className="bg-gray-900 text-gray-400 py-4 text-xs">
    {/* Legal info */}
  </div>
</footer>
```

#### Breadcrumb

**Location:** `components/Breadcrumb.tsx`

```tsx
<nav className="flex" aria-label="Breadcrumb">
  <ol className="inline-flex items-center space-x-1 md:space-x-3">
    <li className="inline-flex items-center">
      <Link className="text-gray-700 hover:text-[#087096]">
        Home
      </Link>
    </li>
    <li>
      <ChevronRightIcon className="w-5 h-5 text-gray-400" />
    </li>
  </ol>
</nav>
```

#### Pagination

**Location:** `components/Pagination.tsx`

```tsx
<nav className="flex items-center space-x-2">
  {/* Previous Button */}
  <Link className="p-2 rounded flex items-center
                   hover:bg-gray-200 text-[#087096]">
    <ChevronLeft />
    <span>Trước</span>
  </Link>

  {/* Page Numbers */}
  <Link className="px-4 py-2 rounded bg-[#087096] text-white">
    {page}
  </Link>

  {/* Next Button */}
  <Link className="p-2 rounded flex items-center
                   hover:bg-gray-200 text-[#087096]">
    <span>Sau</span>
    <ChevronRight />
  </Link>
</nav>
```

**Properties:**
- **Active Page:** `bg-[#087096] text-white`
- **Inactive Page:** `hover:bg-gray-200 text-[#087096]`
- **Spacing:** `space-x-2`
- **Padding:** `px-4 py-2` for page numbers

### Modals & Overlays

#### Success Message (Form Submission)

**Location:** `components/sections/RegistrationForm.tsx:382-402`

```tsx
<div className="bg-white shadow-2xl rounded-xl p-8 lg:p-10
                text-center transition-all duration-500">
  <svg className="w-16 h-16 mx-auto text-green-500">
    {/* Checkmark Icon */}
  </svg>
  <h2 className="text-2xl font-bold text-gray-800 mt-4">
    Cảm ơn bạn!
  </h2>
  <p className="text-gray-600 mt-2">
    {message}
  </p>
</div>
```

---

## Layout & Grid System

### Container System

**Base Container:**
```tsx
<div className="container mx-auto px-4 sm:px-6 lg:px-8">
  {/* Content */}
</div>
```

**Properties:**
- **Max Width:** Defined by Tailwind's container (1280px default)
- **Horizontal Margin:** `mx-auto` (centered)
- **Responsive Padding:**
  - Mobile: `px-4` (16px)
  - Small: `px-6` (24px)
  - Large: `px-8` (32px)

### Grid Layouts

#### Article Grid (2-3-4 columns)

**Location:** `components/sections/HeroFeaturedSection.tsx:121`

```tsx
// 3-column grid (responsive)
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3
                gap-6 md:gap-8">
  {articles.map(article => <PostCard />)}
</div>

// 4-column grid
<div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3
                lg:grid-cols-4 gap-6">
  {articles.map(article => <PostCard />)}
</div>
```

**Breakpoints:**
- **Mobile:** 1 column
- **Small (640px+):** 2 columns
- **Medium (768px+):** 3 columns (optional)
- **Large (1024px+):** 3-4 columns

#### Hero Layout (50/50 Split)

```tsx
<div className="grid grid-cols-1 lg:grid-cols-12 gap-4 md:gap-8">
  <div className="lg:col-span-6">{/* Left content */}</div>
  <div className="lg:col-span-6">{/* Right content */}</div>
</div>
```

#### Footer Grid (4 columns)

```tsx
<div className="grid grid-cols-1 md:grid-cols-4 gap-8">
  {/* Footer columns */}
</div>
```

### Spacing Patterns

#### Section Spacing
```tsx
<section className="py-12 bg-white">        // Default section
<section className="py-16 bg-white">        // Large section
<div className="mb-8 md:mb-12">             // Responsive bottom margin
```

#### Content Spacing
```tsx
<div className="space-y-5">                 // Vertical spacing (forms)
<nav className="space-x-6">                 // Horizontal spacing (nav)
<div className="gap-6 md:gap-8">            // Grid gap (responsive)
```

### Responsive Utilities

#### Show/Hide Elements
```tsx
<button className="md:hidden">              // Mobile only
<nav className="hidden md:flex">            // Desktop only
```

#### Responsive Text
```tsx
<h1 className="text-3xl md:text-4xl">      // Scale up on desktop
<p className="text-sm md:text-base">        // Scale up on desktop
```

#### Responsive Padding/Margin
```tsx
<div className="p-3 md:p-4">               // Increase padding
<div className="mb-4 md:mb-6">              // Increase margin
```

---

## Icons & Images

### Icon Library

**Primary:** **Lucide React** (`lucide-react`)
**Secondary:** **Heroicons** (`@heroicons/react`)

**Installation:**
```bash
npm install lucide-react @heroicons/react
```

**Usage:**
```tsx
import { ChevronLeft, ChevronRight } from "lucide-react";

<ChevronLeft size={20} className="text-[#087096]" />
```

### Image Handling

#### Next.js Image Component

**Always use `next/image`** for optimized loading:

```tsx
import Image from "next/image";

<Image
  src={imageUrl}
  alt="Descriptive alt text"
  fill                           // Responsive fill
  priority                       // Above-fold images
  className="object-cover"       // Crop method
/>
```

#### Image Sizes

```tsx
// Card Images
<div className="relative h-[250px]">
  <Image fill className="object-cover" />
</div>

// Hero Images
<div className="relative h-[450px]">
  <Image fill className="object-cover" />
</div>

// Author Images
<div className="relative h-64 sm:h-56 md:h-64">
  <Image fill className="object-cover" />
</div>
```

#### Image Hover Effects

```tsx
<Image className="object-cover hover:scale-105
                  transition-transform duration-500" />
```

#### Fallback Images

**Location:** `components/PostCard.tsx:13`

```tsx
const DEFAULT_IMAGE = "/images/image.png";

const imageUrl = getImageUrl(post.image) || DEFAULT_IMAGE;
```

### Directus Image URLs

**Helper Function:** `lib/directus/index.ts`

```tsx
export function getImageUrl(imageId: string | null): string | null {
  if (!imageId) return null;
  return `${DIRECTUS_URL}/assets/${imageId}`;
}

// Usage with cache busting
const imageUrl = getImageUrl(post.image) +
                 "?v=" + new Date(post.date_updated).getTime();
```

### Border Radius

```
rounded-none    0px
rounded-sm      2px
rounded         4px
rounded-md      6px
rounded-lg      8px      // Primary (cards, buttons, inputs)
rounded-xl      12px     // Modals, large cards
rounded-2xl     16px
rounded-full    9999px   // Circular (avatars, badges)
```

### Image Alignment (Article Content)

**Location:** `components/content-decoder.css:82-114`

```css
/* Left-aligned image */
.article-content .image-wrap.align-left {
  float: left;
  margin-right: 1.5rem;
  max-width: 50%;
}

/* Right-aligned image */
.article-content .image-wrap.align-right {
  float: right;
  margin-left: 1.5rem;
  max-width: 50%;
}

/* Center-aligned image */
.article-content .image-wrap.align-center {
  display: block;
  text-align: center;
  margin-left: auto;
  margin-right: auto;
}
```

---

## Spacing & Sizing

### Spacing Scale

Tailwind's default spacing scale (1 unit = 0.25rem = 4px):

```
0     0px
px    1px
0.5   2px
1     4px
2     8px
3     12px
4     16px
5     20px
6     24px
8     32px
10    40px
12    48px
16    64px
20    80px
```

### Common Spacing Patterns

#### Component Spacing
```tsx
// Padding
p-3        // 12px (card content mobile)
p-4        // 16px (card content desktop)
p-6        // 24px (modal, large card)
p-8        // 32px (modal, form container)
p-10       // 40px (large modal)

// Margin
mb-2       // 8px (small gap)
mb-4       // 16px (default gap)
mb-6       // 24px (medium gap)
mb-8       // 32px (large gap)
mb-12      // 48px (section gap)
```

#### Vertical Rhythm
```tsx
<div className="space-y-1">    // List items (4px)
<div className="space-y-2">    // Form fields (8px)
<div className="space-y-4">    // Paragraphs (16px)
<div className="space-y-5">    // Form inputs (20px)
<div className="space-y-6">    // Card grid (24px)
<div className="space-y-8">    // Section blocks (32px)
```

### Sizing Guidelines

#### Heights
```tsx
// Fixed Heights
h-16       // 64px (Header/Logo)
h-[250px]  // 250px (Card images)
h-[450px]  // 450px (Hero images)
h-64       // 256px (Author images)

// Min Heights
min-h-screen        // 100vh (Full page)
min-h-[45vh]        // 45vh (CTA section)
```

#### Widths
```tsx
// Container Widths
w-full              // 100%
max-w-[800px]       // 800px (Centered titles)
max-w-none          // No limit (full bleed)

// Component Widths
w-32       // 128px (Logo)
w-64       // 256px (Dropdown menu)
w-96       // 384px (Wider dropdown)
```

### Border Radius System

**Location:** `tailwind.config.ts:17-21`

```typescript
borderRadius: {
  lg: "var(--radius)",              // 8px (default)
  md: "calc(var(--radius) - 2px)",  // 6px
  sm: "calc(var(--radius) - 4px)",  // 4px
}
```

**CSS Variable:**
```css
--radius: 0.5rem;  /* 8px */
```

---

## Interactive States

### Hover States

#### Links
```tsx
<Link className="text-gray-700 hover:text-[#087096]
                 transition-colors">
  Link Text
</Link>
```

#### Buttons
```tsx
<button className="bg-[#fa770c] hover:bg-[#e56c0b]
                   transition-colors duration-300">
  Button
</button>
```

#### Cards
```tsx
<article className="shadow hover:shadow-md transition-shadow">
  {/* Card content */}
</article>
```

#### Images (Scale Effect)
```tsx
<Image className="hover:scale-105 transition-transform duration-500" />
```

### Focus States

#### Inputs
```tsx
<input className="focus:outline-none focus:ring-2
                  focus:ring-blue-500 transition" />
```

#### Buttons
```tsx
<button className="focus:outline-none focus:ring-4
                   focus:ring-orange-300">
  Button
</button>
```

### Active States

#### Navigation
```tsx
// Active page
<Link className="bg-[#087096] text-white">
  Current Page
</Link>

// Inactive page
<Link className="text-[#087096] hover:bg-gray-200">
  Other Page
</Link>
```

### Disabled States

```tsx
<button className="disabled:bg-orange-300
                   disabled:cursor-not-allowed"
        disabled={isLoading}>
  {isLoading ? "ĐANG GỬI..." : "TẢI NGAY"}
</button>
```

### Loading States

```tsx
{isLoading && (
  <div className="flex items-center justify-center">
    <div className="animate-spin rounded-full h-8 w-8
                    border-b-2 border-[#087096]"></div>
  </div>
)}
```

### Transition Patterns

```tsx
// Fast (200ms) - Inputs, small changes
transition-colors duration-200

// Medium (300ms) - Buttons, most interactions
transition-colors duration-300

// Slow (500ms) - Images, dramatic effects
transition-transform duration-500

// Combined
transition-all duration-500
```

---

## Responsive Design

### Breakpoint System

**Tailwind CSS Breakpoints:**

```
sm:   640px   (Small tablets, large phones landscape)
md:   768px   (Tablets)
lg:   1024px  (Small laptops, tablets landscape)
xl:   1280px  (Desktops)
2xl:  1536px  (Large desktops)
```

### Mobile-First Approach

**Always design for mobile first, then scale up:**

```tsx
// ✅ Correct: Mobile first
<div className="text-3xl md:text-4xl">

// ❌ Wrong: Desktop first
<div className="text-4xl md:text-3xl">
```

### Common Responsive Patterns

#### Typography
```tsx
<h1 className="text-3xl md:text-4xl lg:text-5xl">
<p className="text-sm md:text-base lg:text-lg">
```

#### Spacing
```tsx
<div className="p-3 md:p-4 lg:p-6">
<div className="mb-4 md:mb-6 lg:mb-8">
<div className="gap-4 md:gap-6 lg:gap-8">
```

#### Layout
```tsx
// Grid columns
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3">

// Flexbox direction
<div className="flex flex-col md:flex-row">

// Visibility
<button className="md:hidden">              // Mobile only
<nav className="hidden md:flex">            // Desktop only
```

#### Container Padding
```tsx
<div className="container mx-auto px-4 sm:px-6 lg:px-8">
```

### Responsive Image Heights

```tsx
// Card images
<div className="relative h-[250px] sm:h-[250px]">

// Hero images
<div className="relative h-[450px] sm:h-[450px] md:h-[450px]">

// Author images
<div className="relative h-64 sm:h-56 md:h-64">
```

### Mobile Navigation

**Pattern:** Hamburger menu on mobile, horizontal nav on desktop

```tsx
{/* Mobile Menu Button */}
<button className="md:hidden" onClick={toggleMenu}>
  <svg>{/* Menu icon */}</svg>
</button>

{/* Desktop Navigation */}
<nav className="hidden md:flex items-center space-x-6">
  {/* Nav items */}
</nav>

{/* Mobile Navigation */}
{isMenuOpen && (
  <nav className="md:hidden pt-4 pb-2">
    <div className="flex flex-col space-y-1">
      {/* Nav items */}
    </div>
  </nav>
)}
```

### Touch Targets

**Minimum touch target size: 44x44px (iOS) / 48x48px (Android)**

```tsx
// Button padding ensures adequate touch area
<button className="px-6 py-3">  // 48px+ height
<Link className="px-4 py-2">    // 40px height (acceptable)
```

### Responsive Tables

**Location:** `app/globals.css:124-139`

```css
.prose table {
  @apply w-full text-sm;
  overflow-x: auto;
  display: block;
}
```

---

## Code Examples

### Complete Component Example: Article Card

```tsx
import Image from "next/image";
import Link from "next/link";
import type { DirectusArticlePreview } from "@/lib/directus/types";
import { getImageUrl } from "@/lib/directus";

const DEFAULT_IMAGE = "/images/image.png";
const BASE_PATH = process.env.NEXT_PUBLIC_BASE_PATH || "";

interface PostCardProps {
  post: DirectusArticlePreview;
}

export default function PostCard({ post }: PostCardProps) {
  const imageUrl = getImageUrl(post.image) +
                   "?v=" + new Date(post.date_updated).getTime() ||
                   DEFAULT_IMAGE;

  return (
    <article className="group bg-white rounded-lg overflow-hidden
                        shadow hover:shadow-md transition-shadow
                        h-full flex flex-col">
      {/* Article Image */}
      <div className="relative h-[250px] sm:h-[250px] overflow-hidden">
        <Link href={`${BASE_PATH}/${post.slug}`}>
          <Image
            src={imageUrl}
            alt={post.title || "Hình ảnh bài viết"}
            fill
            className="object-cover hover:scale-105
                       transition-transform duration-500"
          />
        </Link>
      </div>

      {/* Article Content */}
      <div className="p-3 md:p-4 flex flex-col flex-grow">
        {/* Category */}
        {post.category && (
          <Link
            href={`${BASE_PATH}/${
              typeof post.category === "object"
                ? post.category.slug
                : post.category
            }`}
            className="text-xs font-medium text-gray-500
                       hover:text-[#fa770c] transition-colors
                       mb-2 inline-block"
          >
            {typeof post.category === "object"
              ? post.category.name
              : post.category}
          </Link>
        )}

        {/* Title */}
        <Link href={`${BASE_PATH}/${post.slug}`}>
          <h3 className="text-lg font-semibold line-clamp-2
                         hover:text-[#fa770c] transition-colors mb-2">
            {post.title}
          </h3>
        </Link>

        {/* Excerpt */}
        {post.excerpt && (
          <p className="text-gray-600 text-sm mb-3 line-clamp-3">
            {post.excerpt}
          </p>
        )}
      </div>
    </article>
  );
}
```

**Key Design Elements:**
1. **Container:** White card with rounded corners and shadow
2. **Hover Effect:** Shadow increases on hover (`.shadow` → `.hover:shadow-md`)
3. **Image:** 250px height, scales to 105% on hover
4. **Layout:** Flexbox column with `flex-grow` for equal heights
5. **Typography:** Small category, large title, gray excerpt
6. **Colors:** Gray text, orange hover, white background
7. **Spacing:** `p-3 md:p-4` responsive padding

---

### Complete Component Example: Primary Button

```tsx
interface ButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  disabled?: boolean;
  type?: "button" | "submit" | "reset";
}

export default function PrimaryButton({
  children,
  onClick,
  disabled = false,
  type = "button",
}: ButtonProps) {
  return (
    <button
      type={type}
      onClick={onClick}
      disabled={disabled}
      className="bg-[#fa770c] text-white px-6 py-3 rounded-lg
                 font-bold hover:bg-[#e56c0b]
                 transition-colors duration-300
                 focus:outline-none focus:ring-4 focus:ring-orange-300
                 disabled:bg-orange-300 disabled:cursor-not-allowed
                 transform hover:scale-105"
    >
      {children}
    </button>
  );
}
```

**Usage:**
```tsx
<PrimaryButton onClick={handleSubmit}>
  TẢI NGAY
</PrimaryButton>

<PrimaryButton disabled={isLoading}>
  {isLoading ? "ĐANG GỬI..." : "TẢI NGAY"}
</PrimaryButton>
```

---

### Complete Component Example: Form Input with Validation

```tsx
import { ChangeEvent } from "react";

interface TextInputProps {
  name: string;
  value: string;
  onChange: (e: ChangeEvent<HTMLInputElement>) => void;
  placeholder?: string;
  type?: "text" | "email" | "tel" | "password";
  error?: string;
}

export default function TextInput({
  name,
  type = "text",
  value,
  onChange,
  placeholder,
  error,
}: TextInputProps) {
  return (
    <div>
      <input
        id={name}
        name={name}
        type={type}
        value={value}
        onChange={onChange}
        placeholder={placeholder}
        className={`shadow-sm appearance-none border rounded-lg
                    w-full py-3 px-4 text-gray-800 leading-tight
                    focus:outline-none focus:ring-2 focus:ring-blue-500
                    transition duration-200
                    ${error ? "border-red-500" : "border-gray-300"}`}
        aria-invalid={error ? "true" : "false"}
        aria-describedby={error ? `${name}-error` : undefined}
      />
      {error && (
        <p id={`${name}-error`}
           className="text-red-500 text-xs italic mt-1"
           role="alert">
          {error}
        </p>
      )}
    </div>
  );
}
```

**Usage:**
```tsx
const [formData, setFormData] = useState({ email: "" });
const [errors, setErrors] = useState({ email: "" });

<TextInput
  name="email"
  type="email"
  value={formData.email}
  onChange={handleChange}
  placeholder="Email công việc"
  error={errors.email}
/>
```

---

### Complete Layout Example: Hero Section

```tsx
import Image from "next/image";
import Link from "next/link";
import { DirectusArticlePreview } from "@/lib/directus/types";
import { getImageUrl } from "@/lib/directus";
import PostCard from "../PostCard";

interface HeroFeaturedSectionProps {
  title: string;
  featuredArticle: DirectusArticlePreview;
  subArticles: DirectusArticlePreview[];
  action_link?: string;
  action_text?: string;
}

export default function HeroFeaturedSection({
  title,
  featuredArticle,
  subArticles = [],
  action_link,
  action_text,
}: HeroFeaturedSectionProps) {
  const imageUrl = getImageUrl(featuredArticle.image) ||
                   "/images/image.png";

  return (
    <section className="py-12 bg-white">
      <div className="container mx-auto px-4">
        {/* Section Title */}
        <div className="flex flex-col items-center mb-10">
          <h1 className="text-3xl md:text-4xl font-bold
                         text-[#087096] text-center
                         w-full max-w-[800px] mb-4 px-4">
            {title}
          </h1>
        </div>

        {/* Featured Article (50/50 Split) */}
        <div className="mb-8 md:mb-12">
          <div className="grid grid-cols-1 lg:grid-cols-12
                          gap-4 md:gap-8 bg-white rounded-lg
                          shadow-md overflow-hidden">
            {/* Left: Image */}
            <div className="lg:col-span-6 relative h-[450px]">
              <Link href={`/${featuredArticle.slug}`}>
                <Image
                  src={imageUrl}
                  alt={featuredArticle.title}
                  fill
                  priority
                  className="object-cover"
                />
              </Link>
            </div>

            {/* Right: Content */}
            <div className="lg:col-span-6 p-4 md:p-6
                            flex flex-col justify-center">
              {/* Category */}
              {featuredArticle.category && (
                <Link
                  href={`/${featuredArticle.category.slug}`}
                  className="text-xs font-medium text-gray-500
                             hover:text-[#fa770c] transition-colors
                             mb-2 inline-block"
                >
                  {featuredArticle.category.name}
                </Link>
              )}

              {/* Title */}
              <Link href={`/${featuredArticle.slug}`}
                    className="block mb-4">
                <h2 className="text-xl sm:text-2xl md:text-3xl
                               font-bold hover:text-[#fa770c]
                               transition-colors">
                  {featuredArticle.title}
                </h2>
              </Link>

              {/* Excerpt */}
              {featuredArticle.excerpt && (
                <p className="text-gray-600 mb-4 md:mb-6
                              line-clamp-3 text-base md:text-lg">
                  {featuredArticle.excerpt}
                </p>
              )}
            </div>
          </div>
        </div>

        {/* Sub Articles Grid */}
        <div className="grid grid-cols-1 sm:grid-cols-2
                        lg:grid-cols-3 gap-6 md:gap-8 mb-6 md:mb-8">
          {subArticles.slice(0, 3).map((article) => (
            <PostCard key={article.id} post={article} />
          ))}
        </div>

        {/* CTA Button */}
        {action_link && action_text && (
          <div className="flex justify-center">
            <Link
              href={action_link}
              className="bg-[#fa770c] text-white px-6 py-2
                         md:px-8 md:py-3 rounded-md font-medium
                         hover:bg-[#e56a0a] transition-colors
                         text-sm md:text-base"
            >
              {action_text}
            </Link>
          </div>
        )}
      </div>
    </section>
  );
}
```

**Key Design Elements:**
1. **Section Container:** `container mx-auto px-4` for responsive padding
2. **Title:** Centered, teal color, max-width constraint
3. **Hero Layout:** 12-column grid with 50/50 split on desktop
4. **Responsive Image:** 450px fixed height
5. **Content Alignment:** Flexbox centering in right column
6. **Sub Articles:** 3-column responsive grid
7. **CTA:** Centered orange button with hover effect

---

## Utility Classes Reference

### Most Commonly Used Classes

```tsx
// Layout
container mx-auto px-4
flex flex-col items-center justify-center
grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3
space-y-4
gap-6 md:gap-8

// Typography
text-3xl md:text-4xl font-bold
text-gray-700 hover:text-[#087096]
line-clamp-2 line-clamp-3

// Colors
bg-white
bg-[#fa770c] hover:bg-[#e56c0b]
text-[#087096]
border-gray-200

// Spacing
p-3 md:p-4
mb-4 md:mb-6
py-12

// Effects
shadow hover:shadow-md
rounded-lg
transition-colors duration-300
hover:scale-105

// Interactive
focus:outline-none focus:ring-2
disabled:bg-orange-300 disabled:cursor-not-allowed
```

---

## Best Practices Summary

### DO's ✅

1. **Use Design Tokens:** Always use CSS custom properties for colors
2. **Mobile-First:** Design for mobile, then scale up
3. **Semantic HTML:** Use proper heading hierarchy (h1 → h6)
4. **Alt Text:** Always provide descriptive alt text for images
5. **Consistent Spacing:** Use the spacing scale (4px increments)
6. **Transitions:** Add transitions to all hover states
7. **Focus States:** Ensure all interactive elements have visible focus
8. **TypeScript:** Use proper types for all component props
9. **Responsive Images:** Use `next/image` with proper sizing
10. **Container Pattern:** Use `container mx-auto px-4` for page width

### DON'Ts ❌

1. **Don't use arbitrary values excessively:** Stick to design system
2. **Don't skip responsive breakpoints:** Test mobile, tablet, desktop
3. **Don't use inline styles:** Use Tailwind classes
4. **Don't hardcode colors:** Use theme colors or brand constants
5. **Don't ignore accessibility:** Test with keyboard navigation
6. **Don't use small touch targets:** Minimum 44x44px
7. **Don't skip loading states:** Always show feedback
8. **Don't forget error states:** Handle all form validation
9. **Don't use `<img>` tags:** Always use `next/image`
10. **Don't ignore performance:** Optimize images and lazy load

---

## Tools & Resources

### Development Tools
- **Tailwind CSS IntelliSense:** VS Code extension
- **Tailwind CSS DevTools:** Browser extension
- **PostCSS:** For Tailwind processing

### Design Resources
- **Tailwind UI:** Component examples
- **Heroicons:** Icon library
- **Lucide Icons:** Alternative icon library

### Accessibility Testing
- **axe DevTools:** Browser extension
- **WAVE:** Web accessibility evaluation tool
- **Lighthouse:** Chrome DevTools audit

### Performance Testing
- **Next.js Analytics:** Built-in performance monitoring
- **WebPageTest:** Performance analysis
- **GTmetrix:** Speed and optimization testing

---

## Changelog

### Version 1.0 (January 2025)
- Initial design guideline creation
- Documented color system, typography, and components
- Added comprehensive code examples
- Established responsive design patterns

---

## Contact & Support

For questions or suggestions regarding this design guideline:

- **Project Repository:** [GitHub URL]
- **Design Team:** design@anphabe.com
- **Documentation:** [Internal wiki or Notion page]

---

**End of Design Guideline**
