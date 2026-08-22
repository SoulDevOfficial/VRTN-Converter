A tiny, zero-backend tool that takes React components written for Vite + `react-router-dom` and rewrites them for the Next.js App Router.

It runs 100% in your browser using Babel AST parsing so no server, no data recording, no API.

# Why I Built This

Porting old Vite pages or Shadcn template components over to Next.js by hand gets tedious fast. Replaces imports, swapping `<Link to>` for `<Link href>`, fixing `useLocation()` returning an object vs `usePathname()` returning a string, and dealing with `navigate(-1)` bugs is boring work. 

This tool automates 95% of that boilerplate so you can just copy, convert, and paste it.

NOTE: This is not meant as a full replacement nor will it get every part right. To be 100% honest, converting it manually **would** be better. This is just an attempt to fix an annoyance I had lmao

# What It Automatically Fixes

* **Imports:** Swaps `react-router-dom` for `next/navigation` and `next/link`.
* **Client Directives:** Prepends `"use client";` at the top if the file uses hooks (`useState`, `useEffect`, `useRouter`, etc.).
* **Router Misuse:** 
  * Renames `useLocation()` to `usePathname()`.
  * Fixes the `location.pathname` bug (since `usePathname()` returns a string directly, not an object).
* **Navigation:** Converted `useNavigate()` to `useRouter()` and turns `navigate(-1)` into `router.back()`.
* **JSX Props:** Swaps `to="..."` for `href="..."` on standard `<Link>` components.
* **Auto-Fixes & Line Logs:** Corrects common typos (like `exsist` -> `exist`) and outputs a log with exact line numbers showing everything it modified.

# How to Run It

Since it's built to run statically, you have two options:

### Option 1: Open locally
Just clone the repo and open `index.html` in your browser.
```bash
git clone [https://github.com/SoulDevOfficial/VRTN-Converter.git](https://github.com/SoulDevOfficial/VRTN-Converter.git)
cd vite-to-next-converter
open index.html
```
 ### Option 2: Use This Repo
 Just click the blue link in the repo description :)

# Things It Won't Do
AST parsing is great for syntax changes, but it can't guess your structure so you'll still need to manually check:

1. Routes / useParams: Next.js App Router passes route params down through page props (({ params })), not through a hook. The log will flag useParams() if it sees it.

2. CSS / Fonts: Any font or CSS imports at the top of your page component should probably be moved to your root app/layout.tsx.

3. Vite Env: It won't automatically rewrite import.meta.env.VITE_* to process.env.NEXT_PUBLIC_* across your project.

# Stack
- HTML + Tailwind CSS (via CDN)
- @babel/standalone (in-browser AST parsing, traversing, and generation)

# License
MIT. Do whatever you want with it.
