
# 2. Quy trình dự án & Coding Guidelines

## 2.1. Cấu trúc thư mục dự án

```
project-root/
├── app/                      # Next.js App Router
│   ├── api/                  # API routes
│   ├── (routes)/             # Route groups
│   ├── layout.tsx            # Root layout
│   ├── globals.css           # Global styles
│   └── not-found.tsx         # 404 page
├── components/               # React components
│   ├── ui/                   # Reusable UI components
│   ├── sections/             # Section components (Hero, Featured, etc.)
│   ├── goc-nhin/            # Feature-specific components
│   └── [ComponentName].tsx   # Page-level components
├── lib/                      # Utilities & helpers
│   ├── directus/             # Directus CMS integration
│   ├── utils/                # Utility functions
│   ├── constants.ts          # App constants
│   └── utils.ts              # Common utilities
├── types/                    # TypeScript type definitions
├── public/                   # Static assets
├── utils/                    # Legacy utilities (đang được migrate)
├── tests/                    # Test files
└── docs/                     # Documentation
    └── rules/         # AI Model guidelines
```

## 2.2. Quy tắc đặt tên

### Variables & Functions

```typescript
// ✅ ĐÚNG - camelCase cho variables, functions
const articleData = fetchArticles();
const isLoading = true;
const handleSubmit = () => {};
const getUserById = (id: string) => {};

// ❌ SAI
const ArticleData = fetchArticles();
const is_loading = true;
const HandleSubmit = () => {};
```
## 2.Linting & Formatting

### ESLint Configuration

Project sử dụng **Next.js ESLint config**:

```json
// .eslintrc.json
{
  "extends": "next/core-web-vitals"
}
```

**Chạy linting:**

```bash
npm run lint
```
### Quy trình Review Code

**Checklist trước khi commit:**

- [ ] Code đã pass `npm run lint` không có errors
- [ ] Code đã được format đúng chuẩn
- [ ] Tất cả TypeScript types đã được định nghĩa rõ ràng
- [ ] Không có `any` types (trừ trường hợp đặc biệt có comment giải thích)
- [ ] Component names theo PascalCase
- [ ] Function/variable names theo camelCase
- [ ] Đã thêm comments cho logic phức tạp
- [ ] Đã test component trong browser (visual check)
- [ ] Responsive design đã được kiểm tra
- [ ] Accessibility đã được kiểm tra (keyboard navigation, ARIA labels)

### Components

```typescript
// ✅ ĐÚNG - PascalCase cho components
export default function ArticleCard({ article }: ArticleCardProps) {}
export function PostCard({ post }: PostCardProps) {}

// ❌ SAI
export default function articleCard() {}
export function post_card() {}
```

### Types & Interfaces

```typescript
// ✅ ĐÚNG - PascalCase, Interface prefix hoặc Type suffix
interface ArticleCardProps {
  article: DirectusArticle;
}

type UserRole = "admin" | "editor" | "viewer";

// ❌ SAI
interface articleCardProps {}
type userRole = string;
```

### Files & Folders

```typescript
// ✅ ĐÚNG
components / ArticleCard.tsx; // Component files: PascalCase
lib / utils / formatDate.ts; // Utility files: camelCase
types / article.ts; // Type files: camelCase
components / sections / FeaturedSection.tsx; // Folders: camelCase hoặc kebab-case

// ❌ SAI
components / article - card.tsx;
lib / utils / FormatDate.ts;
types / Article.ts;
```

### Constants

```typescript
// ✅ ĐÚNG - UPPER_SNAKE_CASE cho constants
const BASE_PATH = process.env.NEXT_PUBLIC_BASE_PATH || "";
const MAX_ARTICLES_PER_PAGE = 10;
const API_ENDPOINTS = {
  ARTICLES: "/api/articles",
  CATEGORIES: "/api/categories",
};

// ❌ SAI
const basePath = "/research-insights";
const maxArticlesPerPage = 10;
```

## 2.3. Quy tắc phân chia Component

### Nguyên tắc tổ chức

```typescript
// 1. Small & Focused - Mỗi component chỉ làm một việc
// ✅ ĐÚNG
function ArticleTitle({ title }: { title: string }) {
  return <h1 className="text-3xl font-bold">{title}</h1>;
}

function ArticleImage({ src, alt }: { src: string; alt: string }) {
  return <Image src={src} alt={alt} fill className="object-cover" />;
}

// ❌ SAI - Component quá lớn, làm nhiều việc
function ArticleHeader({ article }) {
  return (
    <div>
      <h1>{article.title}</h1>
      <Image src={article.image} alt={article.title} />
      <div>{article.author}</div>
      <div>{article.date}</div>
      <div>{article.category}</div>
    </div>
  );
}
```

### Phân loại Components

```typescript
// 1. UI Components (components/ui/) - Reusable, generic
components/ui/
├── TextInput.tsx         // Form input
├── Checkbox.tsx          // Checkbox control
├── SelectInput.tsx       // Select dropdown
└── types.ts              // Shared types

// 2. Section Components (components/sections/) - Layout sections
components/sections/
├── HeroFeaturedSection.tsx      // Hero banner
├── FeaturedSection.tsx          // Featured articles
├── ArticlesGridSection.tsx      // Articles grid
└── RegistrationForm.tsx         // Registration section

// 3. Feature Components (components/[feature]/) - Feature-specific
components/goc-nhin/
├── AuthorCard.tsx              // Author card
├── ArticlesList.tsx            // Articles list
└── AuthorInfo.tsx              // Author info

// 4. Page-level Components (components/) - Kết hợp nhiều components
components/
├── ArticlePageClient.tsx       // Article page wrapper
├── PostCard.tsx                // Post card component
└── Header.tsx                  // Site header
```

## 2.4. TypeScript Guidelines

### Type Safety

```typescript
// ✅ ĐÚNG - Luôn định nghĩa types rõ ràng
interface ArticleCardProps {
  article: DirectusArticle;
  showExcerpt?: boolean;
  onCardClick?: (id: string) => void;
}

export function ArticleCard({
  article,
  showExcerpt = true,
  onCardClick,
}: ArticleCardProps) {
  // Component implementation
}

// ❌ SAI - Tránh dùng 'any'
function ArticleCard({ article }: { article: any }) {
  // Bad practice
}
```

### Type Imports

```typescript
// ✅ ĐÚNG - Import types từ thư mục types hoặc lib
import type {
  DirectusArticle,
  DirectusArticlePreview,
} from "@/lib/directus/types";
import type { Metadata } from "next";

// ✅ ĐÚNG - Type-only imports
import type { FC } from "react";

// ❌ SAI - Import cả implementation khi chỉ cần type
import { DirectusArticle } from "@/lib/directus/types"; // Nếu chỉ dùng như type
```

## 3.2. Styling - Tailwind CSS

### Utility-First Approach

```tsx
// ✅ ĐÚNG - Sử dụng Tailwind utilities
<div className="flex items-center gap-4 p-6 bg-white rounded-lg shadow-md hover:shadow-lg transition-shadow">
  <h2 className="text-2xl font-bold text-gray-900">Tiêu đề</h2>
</div>

// ❌ SAI - Tránh inline styles khi có thể dùng Tailwind
<div style={{ display: 'flex', padding: '24px' }}>
  <h2 style={{ fontSize: '24px', fontWeight: 'bold' }}>Tiêu đề</h2>
</div>
```

### Responsive Design

```tsx
// ✅ ĐÚNG - Mobile-first approach
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  {/* Responsive grid: 1 col mobile, 2 cols tablet, 3 cols desktop */}
</div>

<h1 className="text-xl md:text-3xl lg:text-5xl font-bold">
  {/* Responsive typography */}
</h1>
```

### Custom Utilities - cn() helper

```tsx
import { cn } from "@/lib/utils";

// ✅ ĐÚNG - Sử dụng cn() để merge class names
<button
  className={cn(
    "px-4 py-2 rounded-lg font-medium transition-colors",
    isActive && "bg-blue-500 text-white",
    isDisabled && "opacity-50 cursor-not-allowed"
  )}
>
  Click me
</button>;
```

### Tailwind Config - Custom Theme

```typescript
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        primary: "hsl(var(--primary))",
        secondary: "hsl(var(--secondary))",
        // ... custom colors
      },
      fontSize: {
        xs: ["0.75rem", { lineHeight: "1rem" }], // 12px
        sm: ["0.875rem", { lineHeight: "1.25rem" }], // 14px
        base: ["1rem", { lineHeight: "1.5rem" }], // 16px
        // ... custom sizes
      },
    },
  },
};
```


### 3.4. Component Creation Guidelines

#### Khi nào tạo component mới?

**✅ NÊN tạo component mới khi:**

- UI pattern được sử dụng ở 3+ nơi khác nhau
- Component có logic phức tạp (>100 dòng)
- Component có thể tái sử dụng với props khác nhau
- Cần tách biệt concerns (UI vs Logic)

**❌ KHÔNG NÊN tạo component mới khi:**

- Chỉ dùng 1-2 lần và rất specific
- Component quá đơn giản (<10 dòng)
- Đã có component tương tự, chỉ cần customize props

#### Template tạo component mới

````tsx
// components/ExampleComponent.tsx
"use client"; // Chỉ thêm nếu cần client-side features

import React from "react";
import { cn } from "@/lib/utils";

/**
 * ExampleComponent - Mô tả ngắn gọn component làm gì
 *
 * @example
 * ```tsx
 * <ExampleComponent
 *   title="Hello"
 *   onAction={() => console.log('clicked')}
 * />
 * ```
 */

interface ExampleComponentProps {
  /** Title to display */
  title: string;
  /** Optional description */
  description?: string;
  /** Click handler */
  onAction?: () => void;
  /** Additional CSS classes */
  className?: string;
}

export default function ExampleComponent({
  title,
  description,
  onAction,
  className,
}: ExampleComponentProps) {
  return (
    <div className={cn("p-4 bg-white rounded-lg", className)}>
      <h2 className="text-2xl font-bold">{title}</h2>
      {description && <p className="text-gray-600">{description}</p>}
      {onAction && (
        <button
          onClick={onAction}
          className="mt-4 px-4 py-2 bg-blue-500 text-white rounded"
        >
          Action
        </button>
      )}
    </div>
  );
}
````

#### Documenting Props & State

**Props Documentation:**

```tsx
interface ComponentProps {
  /**
   * Title của component
   * @example "My Article Title"
   */
  title: string;

  /**
   * Callback khi user click vào component
   * @param id - Article ID
   */
  onClick?: (id: string) => void;

  /**
   * Hiển thị loading state
   * @default false
   */
  isLoading?: boolean;
}
```

**State Documentation:**

```tsx
function MyComponent() {
  // Current page number, starts at 1
  const [page, setPage] = useState(1);

  // Articles loaded from API, undefined while loading
  const [articles, setArticles] = useState<Article[] | undefined>();

  // Error message if fetch fails
  const [error, setError] = useState<string | null>(null);
}
```

---

## 4. Design Patterns & Best Practices

### 4.1. React Patterns được sử dụng

#### 4.1.1. Custom Hooks Pattern

**Khi nào dùng:** Tái sử dụng logic giữa các components

```tsx
// lib/hooks/useArticles.ts
import { useState, useEffect } from "react";
import type { DirectusArticle } from "@/lib/directus/types";

/**
 * Hook để fetch articles với pagination
 */
export function useArticles(page: number = 1, limit: number = 10) {
  const [articles, setArticles] = useState<DirectusArticle[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    async function fetchArticles() {
      try {
        setLoading(true);
        const response = await fetch(
          `/api/articles?page=${page}&limit=${limit}`
        );
        const data = await response.json();
        setArticles(data.articles);
      } catch (err) {
        setError(err as Error);
      } finally {
        setLoading(false);
      }
    }

    fetchArticles();
  }, [page, limit]);

  return { articles, loading, error };
}

// Sử dụng:
function ArticlesList() {
  const { articles, loading, error } = useArticles(1, 10);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      {articles.map((article) => (
        <PostCard key={article.id} post={article} />
      ))}
    </div>
  );
}
```

#### 4.1.2. Container/Presenter Pattern

**Khi nào dùng:** Tách biệt logic và UI

```tsx
// Container Component (Smart Component) - Xử lý logic
"use client";

import { useState, useEffect } from "react";
import ArticlesListPresenter from "./ArticlesListPresenter";

export default function ArticlesListContainer() {
  const [articles, setArticles] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Fetch logic
    fetchArticles().then((data) => {
      setArticles(data);
      setLoading(false);
    });
  }, []);

  const handleArticleClick = (id: string) => {
    // Handle click logic
    console.log("Clicked:", id);
  };

  return (
    <ArticlesListPresenter
      articles={articles}
      loading={loading}
      onArticleClick={handleArticleClick}
    />
  );
}

// Presenter Component (Dumb Component) - Chỉ render UI
interface ArticlesListPresenterProps {
  articles: Article[];
  loading: boolean;
  onArticleClick: (id: string) => void;
}

export default function ArticlesListPresenter({
  articles,
  loading,
  onArticleClick,
}: ArticlesListPresenterProps) {
  if (loading) {
    return <div>Loading...</div>;
  }

  return (
    <div className="grid grid-cols-3 gap-4">
      {articles.map((article) => (
        <div key={article.id} onClick={() => onArticleClick(article.id)}>
          <PostCard post={article} />
        </div>
      ))}
    </div>
  );
}
```

#### 4.1.3. Compound Component Pattern

**Khi nào dùng:** Component phức tạp với nhiều sub-components

```tsx
// components/Card.tsx
import React, { createContext, useContext } from "react";

interface CardContextValue {
  variant: "default" | "featured";
}

const CardContext = createContext<CardContextValue>({ variant: "default" });

function Card({
  children,
  variant = "default",
}: {
  children: React.ReactNode;
  variant?: "default" | "featured";
}) {
  return (
    <CardContext.Provider value={{ variant }}>
      <div className={`card card-${variant}`}>{children}</div>
    </CardContext.Provider>
  );
}

function CardHeader({ children }: { children: React.ReactNode }) {
  const { variant } = useContext(CardContext);
  return <div className={`card-header card-header-${variant}`}>{children}</div>;
}

function CardBody({ children }: { children: React.ReactNode }) {
  return <div className="card-body">{children}</div>;
}

function CardFooter({ children }: { children: React.ReactNode }) {
  return <div className="card-footer">{children}</div>;
}

// Export as compound component
Card.Header = CardHeader;
Card.Body = CardBody;
Card.Footer = CardFooter;

export default Card;

// Sử dụng:
<Card variant="featured">
  <Card.Header>
    <h2>Title</h2>
  </Card.Header>
  <Card.Body>
    <p>Content here</p>
  </Card.Body>
  <Card.Footer>
    <button>Read More</button>
  </Card.Footer>
</Card>;
```

#### 4.1.4. Render Props Pattern (khi cần)

```tsx
interface DataFetcherProps<T> {
  url: string;
  children: (
    data: T | null,
    loading: boolean,
    error: Error | null
  ) => React.ReactNode;
}

function DataFetcher<T>({ url, children }: DataFetcherProps<T>) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((err) => {
        setError(err);
        setLoading(false);
      });
  }, [url]);

  return <>{children(data, loading, error)}</>;
}

// Sử dụng:
<DataFetcher<Article[]> url="/api/articles">
  {(articles, loading, error) => {
    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error.message}</div>;
    return (
      <div>
        {articles?.map((article) => (
          <PostCard key={article.id} post={article} />
        ))}
      </div>
    );
  }}
</DataFetcher>;
```
---

#### 4.2.2. Data Fetching Patterns

```tsx
// ✅ Server Component - Direct data fetching
export default async function Page() {
  const data = await fetch("https://api.example.com/data");
  const articles = await data.json();

  return <ArticlesList articles={articles} />;
}

// ✅ Server Component - With Directus
import { getArticles } from "@/lib/directus";

export default async function Page() {
  const articles = await getArticles();
  return <ArticlesList articles={articles} />;
}

// ✅ Client Component - With SWR or React Query (if needed)
("use client");
import useSWR from "swr";

export default function Page() {
  const { data, error, isLoading } = useSWR("/api/articles", fetcher);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error</div>;

  return <ArticlesList articles={data} />;
}
```

### 4.3. Performance Best Practices

#### 4.3.1. Image Optimization

```tsx
import Image from 'next/image';

// ✅ ĐÚNG - Sử dụng Next.js Image component
<Image
  src={imageUrl}
  alt="Article image"
  width={800}
  height={600}
  priority={isFeatured}  // Priority loading cho hero images
  loading={isFeatured ? undefined : "lazy"}  // Lazy load nếu không phải featured
  className="object-cover"
/>

// ✅ ĐÚNG - Fill mode cho responsive images
<div className="relative h-64">
  <Image
    src={imageUrl}
    alt="Article image"
    fill
    className="object-cover"
    sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
  />
</div>

// ❌ SAI - Dùng <img> tag thông thường
<img src={imageUrl} alt="Article" />
```

#### 4.3.2. Code Splitting & Dynamic Imports

```tsx
import dynamic from "next/dynamic";

// ✅ ĐÚNG - Dynamic import cho heavy components
const HeavyComponent = dynamic(() => import("@/components/HeavyComponent"), {
  loading: () => <div>Loading...</div>,
  ssr: false, // Disable SSR nếu component chỉ cần client-side
});

// ✅ ĐÚNG - Lazy load charts, editors
const ChartComponent = dynamic(() => import("@/components/Chart"), {
  ssr: false,
});

export default function Page() {
  return (
    <div>
      <h1>Page Title</h1>
      <HeavyComponent />
    </div>
  );
}
```

#### 4.3.3. Memoization

```tsx
import { useMemo, useCallback } from "react";

function ArticlesList({ articles, filters }) {
  // ✅ Memoize expensive calculations
  const filteredArticles = useMemo(() => {
    return articles.filter((article) => {
      return filters.category ? article.category === filters.category : true;
    });
  }, [articles, filters.category]);

  // ✅ Memoize callbacks
  const handleArticleClick = useCallback((id: string) => {
    console.log("Clicked:", id);
    // Handle click
  }, []);

  return (
    <div>
      {filteredArticles.map((article) => (
        <PostCard
          key={article.id}
          post={article}
          onClick={handleArticleClick}
        />
      ))}
    </div>
  );
}
```

### 4.4. Accessibility (a11y) Checklist

```tsx
// ✅ Semantic HTML
<article>
  <h1>Article Title</h1>
  <p>Content...</p>
</article>

// ✅ ARIA labels
<button aria-label="Close dialog" onClick={onClose}>
  <XIcon />
</button>

// ✅ Keyboard navigation
<div
  role="button"
  tabIndex={0}
  onClick={handleClick}
  onKeyDown={(e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      handleClick();
    }
  }}
>
  Clickable div
</div>

// ✅ Alt text cho images
<Image
  src={article.image}
  alt={`Featured image for ${article.title}`}
  width={800}
  height={600}
/>

// ✅ Form labels
<label htmlFor="email">Email Address</label>
<input id="email" type="email" name="email" />

// ✅ Focus states
<button className="focus:ring-2 focus:ring-blue-500 focus:outline-none">
  Click me
</button>
```

**Accessibility Checklist:**

- [ ] Tất cả interactive elements có thể access qua keyboard (Tab, Enter, Space)
- [ ] Images có alt text mô tả nội dung
- [ ] Form inputs có labels hoặc aria-label
- [ ] Color contrast ratio đạt WCAG AA (4.5:1 cho text, 3:1 cho UI)
- [ ] Focus states rõ ràng cho tất cả interactive elements
- [ ] Semantic HTML được sử dụng đúng (<article>, <nav>, <main>, etc.)
- [ ] Headings theo thứ tự logic (h1 -> h2 -> h3, không skip levels)
- [ ] ARIA attributes được sử dụng khi cần (aria-label, aria-describedby, role)

### 4.5. SEO Best Practices

```tsx
// app/articles/[slug]/page.tsx
import type { Metadata } from "next";

// ✅ Static Metadata
export const metadata: Metadata = {
  title: "Articles | Research Insights",
  description: "Browse our latest research articles",
};

// ✅ Dynamic Metadata
export async function generateMetadata({ params }): Promise<Metadata> {
  const article = await getArticle(params.slug);

  return {
    title: article.title,
    description: article.excerpt,
    openGraph: {
      title: article.title,
      description: article.excerpt,
      images: [article.image],
      type: "article",
      publishedTime: article.date_created,
      modifiedTime: article.date_updated,
    },
    twitter: {
      card: "summary_large_image",
      title: article.title,
      description: article.excerpt,
      images: [article.image],
    },
  };
}

// ✅ Structured Data (JSON-LD)
export default function ArticlePage({ article }) {
  const jsonLd = {
    "@context": "https://schema.org",
    "@type": "Article",
    headline: article.title,
    description: article.excerpt,
    image: article.image,
    datePublished: article.date_created,
    dateModified: article.date_updated,
    author: {
      "@type": "Person",
      name: article.author.name,
    },
  };

  return (
    <>
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
      />
      <article>{/* Article content */}</article>
    </>
  );
}
```

**SEO Checklist:**

- [ ] Metadata được định nghĩa cho mọi page (title, description)
- [ ] Open Graph tags cho social sharing
- [ ] Canonical URLs cho duplicate content
- [ ] Sitemap.xml được generate
- [ ] Robots.txt được cấu hình đúng
- [ ] Structured data (JSON-LD) cho rich results
- [ ] Image alt text cho SEO
- [ ] Internal linking structure hợp lý
- [ ] Mobile-friendly (responsive design)
- [ ] Page load speed optimization

### 4.6. Error Handling Patterns

```tsx
// ✅ Error Boundaries (app/error.tsx)
"use client";

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div className="flex flex-col items-center justify-center min-h-screen">
      <h2 className="text-2xl font-bold mb-4">Something went wrong!</h2>
      <p className="text-gray-600 mb-4">{error.message}</p>
      <button
        onClick={reset}
        className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
      >
        Try again
      </button>
    </div>
  );
}

// ✅ Try-Catch in Server Components
export default async function Page() {
  try {
    const articles = await getArticles();
    return <ArticlesList articles={articles} />;
  } catch (error) {
    console.error("Failed to fetch articles:", error);
    return <div>Failed to load articles. Please try again later.</div>;
  }
}

// ✅ Error handling in Client Components
("use client");

export default function Component() {
  const [error, setError] = useState<string | null>(null);

  const handleSubmit = async (data) => {
    try {
      setError(null);
      await submitForm(data);
    } catch (err) {
      setError(err instanceof Error ? err.message : "An error occurred");
    }
  };

  return (
    <div>
      {error && (
        <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded">
          {error}
        </div>
      )}
      <form onSubmit={handleSubmit}>{/* Form fields */}</form>
    </div>
  );
}
```
