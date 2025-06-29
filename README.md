# Portfolio Site

A minimal, fast, and framework-free portfolio site built with progressive enhancement in mind.

This site avoids bloated modern frameworks in favor of small, composable tools like [htmx](https://htmx.org), keeping performance and simplicity as top priorities.

---

## Philosophy

This project reflects my personal preference for:

- ✅ Simplicity over complexity  
- ⚡ Speed over trendiness  
- 🧱 HTML-first, progressively enhanced development  
- 🧰 Small tools with clear purpose over large monoliths  

---

## Project Structure

```

/
├── index.html         # Main entry point
├── about.html         # Additional content pages
├── css/
│   └── style.css      # Custom styles (can include Tailwind via CDN if needed)
└── js/                # Optional scripts

````

---

## Getting Started

No build step required. Just open `index.html` in your browser or serve with a static file server:

```bash
# using Python 3
python3 -m http.server

# or using Node
npx serve
````

---

## Tools Used

* [htmx](https://htmx.org) – Load content dynamically using plain HTML and attributes

---

## Why Not a Framework?

Modern JavaScript frameworks like React or Svelte are powerful, but they often add unnecessary complexity and JavaScript overhead for static sites or content-driven projects.

This site embraces a simpler model — where HTML does most of the work.

---

## License

MIT – [LICENSE](LICENSE)

---
