# CRUSH.md - Codebase Guidelines for AI Agents

## 1. Build and Dev Commands

- **Run dev server**: `npm run dev`
- **Build**: `npm run build`
- **Start production server**: `npm run start`
- **Lint**: `npm run lint`
- **Typecheck**: `npm run typecheck`
- **Run all tests**: `npm run test`
- **Run e2e tests**: `npm run test:e2e`
- **Run e2e tests (UI mode)**: `npm run test:e2e:ui`
- **Run e2e tests (headed)**: `npm run test:e2e:headed`
- **Run e2e tests (debug)**: `npm run test:e2e:debug`
- **Show e2e test report**: `npm run test:e2e:report`
- **Run a single test (Vitest)**: `npx vitest --run --reporter=verbose [path/to/test-file]`

# AI Model Onboarding Guide - Research Insights Project

> **Tài liệu hướng dẫn toàn diện cho AI Model làm việc với dự án Research Insights**
>
> Phiên bản: 1.0 | Cập nhật: 2025-10-09

---

## 📋 Mục lục

1. [Môi trường & Công cụ](#1-môi-trường--công-cụ)

---

## 1. Môi trường & Công cụ

### 1.1. Yêu cầu phần mềm bắt buộc

| Công cụ        | Phiên bản yêu cầu              | Ghi chú                        |
| -------------- | ------------------------------ | ------------------------------ |
| **Node.js**    | >= 18.x                        | LTS version được khuyến nghị   |
| **npm**        | >= 9.x hoặc **yarn** >= 1.22.x | Package manager                |
| **TypeScript** | 5.2.2                          | Đã được cấu hình trong project |
| **Next.js**    | ^15.2.4                        | Framework chính của dự án      |
| **React**      | 18.2.0                         | UI Library                     |
| **VS Code**    | Latest                         | Editor khuyên dùng             |

### 1.2. VS Code Extensions khuyên dùng

```json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "styled-components.vscode-styled-components",
    "ms-vscode.vscode-typescript-next"
  ]
}
```

### 1.3. Cài đặt dự án

#### Bước 1: Clone repository

```bash
git clone <repository-url>
cd research-insights
```

#### Bước 2: Cài đặt dependencies

```bash
npm install
# hoặc
yarn install
```

#### Bước 3: Cấu hình file môi trường

Tạo file `.env.local` từ template:

```bash
cp .env.development .env.local
```

**Các biến môi trường quan trọng:**

```env
# Directus CMS Configuration
NEXT_PUBLIC_DIRECTUS_URL=https://your-directus-url.com
DIRECTUS_TOKEN=your-directus-token

# Application Configuration
NEXT_PUBLIC_BASE_PATH=/research-insights
NEXT_PUBLIC_GOOGLE_TAG_ID=GTM-XXXXXXX

# Build Configuration
USE_DIRECTUS=true  # true để sử dụng Directus CMS, false để sử dụng dữ liệu local
```

#### Bước 4: Chạy development server

```bash
# Chạy với Directus CMS
npm run dev:directus

# Chạy với dữ liệu local
npm run dev:local

# Hoặc chạy bình thường (dựa vào .env)
npm run dev
```

Server sẽ chạy tại: `http://localhost:3000`

#### Bước 5: Chạy tests

```bash
# Unit tests với Vitest
npm run test

# E2E tests với Playwright
npm run test:e2e

# E2E tests với UI mode
npm run test:e2e:ui
```
---

---

## 3. Thư viện UI & Components

### 3.1. Tech Stack chính

| Thư viện            | Version | Mục đích sử dụng        |
| ------------------- | ------- | ----------------------- |
| **Next.js**         | 15.2.4  | Framework, SSR, Routing |
| **React**           | 18.2.0  | UI Library              |
| **TypeScript**      | 5.2.2   | Type safety             |
| **Tailwind CSS**    | 3.3.3   | Styling system          |
| **Heroicons**       | 2.2.0   | Icon library            |
| **Lucide React**    | 0.446.0 | Icon library            |
| **Directus SDK**    | 11.0.3  | CMS integration         |
| **React Hook Form** | 7.53.0  | Form handling           |
| **date-fns**        | 3.6.0   | Date formatting         |



---

### 4.2. Next.js Specific Patterns

#### 4.2.1. Server vs Client Components

```tsx
// ✅ Server Component (default) - Không cần 'use client'
// app/articles/page.tsx
import { getArticles } from "@/lib/directus";
import PostCard from "@/components/PostCard";

export default async function ArticlesPage() {
  // Fetch data on server
  const articles = await getArticles();

  return (
    <div>
      {articles.map((article) => (
        <PostCard key={article.id} post={article} />
      ))}
    </div>
  );
}

// ✅ Client Component - Cần 'use client' khi dùng hooks, event handlers
// components/ArticlePageClient.tsx
("use client");

import { useState, useEffect } from "react";

export default function ArticlePageClient({ article }) {
  const [isLiked, setIsLiked] = useState(false);

  // Client-side interactivity
  const handleLike = () => {
    setIsLiked(!isLiked);
  };

  return (
    <div>
      <h1>{article.title}</h1>
      <button onClick={handleLike}>{isLiked ? "Unlike" : "Like"}</button>
    </div>
  );
}
```
---
