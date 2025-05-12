---
icon: python
---

# TailwindCSS X viteReacts

Installing Tailwind CSS as a Vite plugin is the most seamless way to integrate it with frameworks like Laravel, SvelteKit, React Router, Nuxt, and SolidJS.

```
npm install tailwindcss @tailwindcss/vite
```

### Import Tailwindcss Css

Add an `@import` to your CSS file that imports Tailwind CSS.

```
@import "tailwindcss";
```

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Adding

{% code title="vite.config.js" overflow="wrap" lineNumbers="true" %}
```
import path from "path"
import tailwindcss from "@tailwindcss/vite"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
})
```
{% endcode %}
