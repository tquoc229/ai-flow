### 3.3. Components Library - Reusable Components

#### 3.3.1. Form Components (components/ui/)

**TextInput** - Text input field

```tsx
import { TextInput } from "@/components/ui/TextInput";

<TextInput
  name="email"
  label="Email Address"
  type="email"
  placeholder="your@email.com"
  required
  error={errors.email?.message}
/>;
```

**Props:**

- `name: string` - Input name
- `label?: string` - Label text
- `type?: string` - Input type (text, email, password, etc.)
- `placeholder?: string` - Placeholder text
- `required?: boolean` - Required field
- `error?: string` - Error message
- `...rest` - Standard input props

---

**TextareaInput** - Textarea field

```tsx
import { TextareaInput } from "@/components/ui/TextareaInput";

<TextareaInput
  name="message"
  label="Message"
  placeholder="Enter your message..."
  rows={5}
  required
/>;
```

**Props:**

- `name: string` - Input name
- `label?: string` - Label text
- `rows?: number` - Number of rows
- `...rest` - Standard textarea props

---

**SelectInput** - Dropdown select

```tsx
import { SelectInput } from "@/components/ui/SelectInput";

<SelectInput name="category" label="Category" required>
  <option value="">Select a category</option>
  <option value="tech">Technology</option>
  <option value="design">Design</option>
</SelectInput>;
```

**Props:**

- `name: string` - Select name
- `label?: string` - Label text
- `children: ReactNode` - Option elements
- `...rest` - Standard select props

---

**Checkbox** - Single checkbox

```tsx
import { Checkbox } from "@/components/ui/Checkbox";

<Checkbox name="terms" label="I agree to the terms and conditions" required />;
```

**Props:**

- `name: string` - Checkbox name
- `label?: string` - Label text
- `...rest` - Standard checkbox props

---

**Radio** - Radio button

```tsx
import { Radio } from "@/components/ui/Radio";

<Radio name="gender" value="male" label="Male" />;
```

**Props:**

- `name: string` - Radio name (same for group)
- `value: string` - Radio value
- `label?: string` - Label text
- `...rest` - Standard radio props

---

**FileInput** - File upload

```tsx
import { FileInput } from "@/components/ui/FileInput";

<FileInput name="avatar" label="Upload Avatar" accept="image/*" />;
```

**Props:**

- `name: string` - Input name
- `label?: string` - Label text
- `accept?: string` - Accepted file types
- `...rest` - Standard file input props

---

#### 3.3.2. Content Components

**PostCard** - Article card preview

```tsx
import PostCard from "@/components/PostCard";

<PostCard post={article} />;
```

**Props:**

- `post: DirectusArticlePreview` - Article data object
  - `title: string` - Article title
  - `slug: string` - URL slug
  - `excerpt?: string` - Article excerpt
  - `image?: string` - Featured image ID
  - `category: string | Category` - Category info
  - `date_updated: string` - Last update date

**Khi nào dùng:** Hiển thị article preview trong grid, list, related articles

---

**ClientDate** - Formatted date display (Client-side)

```tsx
import ClientDate from "@/components/ClientDate";

<ClientDate date={article.date_updated} />;
```

**Props:**

- `date: string | Date` - Date to format

**Khi nào dùng:** Hiển thị ngày tháng với locale Vietnam (client-side rendering)

---

**Pagination** - Page navigation

```tsx
import Pagination from "@/components/Pagination";

<Pagination
  currentPage={currentPage}
  totalPages={totalPages}
  onPageChange={(page) => router.push(`?page=${page}`)}
/>;
```

**Props:**

- `currentPage: number` - Current page number
- `totalPages: number` - Total number of pages
- `onPageChange: (page: number) => void` - Page change handler

---

**Breadcrumb** - Navigation breadcrumb

```tsx
import Breadcrumb from "@/components/Breadcrumb";

<Breadcrumb
  items={[
    { label: "Home", href: "/" },
    { label: "Category", href: "/category" },
    { label: "Article", href: "/article" },
  ]}
/>;
```

**Props:**

- `items: Array<{ label: string; href: string }>` - Breadcrumb items

---

**TableOfContents** - Article TOC

```tsx
import TableOfContents from "@/components/TableOfContents";

<TableOfContents headings={headings} />;
```

**Props:**

- `headings: Heading[]` - Array of headings
  - `id: string` - Heading ID
  - `text: string` - Heading text
  - `level: number` - Heading level (1-6)

**Khi nào dùng:** Hiển thị mục lục cho bài viết dài

---

#### 3.3.3. Content Rendering Components

**ClientContentRenderer** - Render HTML or Markdown

```tsx
import ClientContentRenderer from "@/components/ClientContentRenderer";

<ClientContentRenderer content={article.content} isHtml={true} />;
```

**Props:**

- `content: string` - Content to render
- `isHtml: boolean` - Whether content is HTML (true) or Markdown (false)

**Khi nào dùng:** Render nội dung bài viết từ CMS (hỗ trợ cả HTML và Markdown)

---

**MarkdownRenderer** - Render Markdown only

```tsx
import MarkdownRenderer from "@/components/MarkdownRenderer";

<MarkdownRenderer content={markdownContent} />;
```

**Props:**

- `content: string` - Markdown content

---

**HtmlRenderer** - Render sanitized HTML

```tsx
import HtmlRenderer from "@/components/HtmlRenderer";

<HtmlRenderer html={htmlContent} />;
```

**Props:**

- `html: string` - HTML content (will be sanitized)

---

#### 3.3.4. Section Components (components/sections/)

**HeroFeaturedSection** - Hero banner with featured article

```tsx
import HeroFeaturedSection from "@/components/sections/HeroFeaturedSection";

<HeroFeaturedSection article={featuredArticle} />;
```

**Props:**

- `article: DirectusArticle` - Featured article data

**Khi nào dùng:** Hero section trên homepage

---

**FeaturedSection** - Featured articles grid

```tsx
import FeaturedSection from "@/components/sections/FeaturedSection";

<FeaturedSection articles={featuredArticles} />;
```

**Props:**

- `articles: DirectusArticle[]` - Array of featured articles

---

**ArticlesGridSection** - Articles grid layout

```tsx
import ArticlesGridSection from "@/components/sections/ArticlesGridSection";

<ArticlesGridSection articles={articles} title="Latest Articles" />;
```

**Props:**

- `articles: DirectusArticle[]` - Articles to display
- `title?: string` - Section title

---

**RegistrationForm** - User registration form

```tsx
import RegistrationForm from "@/components/sections/RegistrationForm";

<RegistrationForm onSubmit={(data) => handleRegistration(data)} />;
```

**Props:**

- `onSubmit: (data: FormData) => void` - Form submit handler

---

#### 3.3.5. Layout Components

**Header** - Site header with navigation

```tsx
import Header from "@/components/Header";

<Header />;
```

**Khi nào dùng:** Main navigation header (đã được include trong layout)

---

**Footer** - Site footer

```tsx
import Footer from "@/components/Footer";

<Footer />;
```

**Khi nào dùng:** Site footer (đã được include trong layout)

---

**Logo** - Site logo component

```tsx
import Logo from "@/components/Logo";

<Logo />;
```
