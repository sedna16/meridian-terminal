# Meridian Terminal v1.0
**A terminal-style personal dashboard for your professional workflow.**

[**Live Demo**](http://localhost:5173/?session=demo-n81ji5cu2h9-99pmq0rcazo-41zlamv253a-luynkpjj92)

---

## What is it?
Meridian Terminal is an open-source, single-page command center. It pulls your project updates, security alerts, and real-time data into one clean, terminal-inspired interface.

* **No account needed:** Your layout is saved to your browser session.
* **Easy Setup:** Just configure your widgets and bookmark the page to save your progress.
* **Privacy Warning:** Do **not** post sensitive or private information on the dashboard.

## Key Features
* **Unified View:** Everything you need on one single page.
* **Lightweight:** Minimalist design without the bloat of a traditional GUI.
* **Fast Customization:** Changes are saved via sessions; no database login required.

---

## Getting Started

### 1. Project Setup
`npm install`

### 2. Environment Variables
Create a `.env` file and add your keys:
* `VITE_SUPABASE_URL`
* `VITE_SUPABASE_ANON_KEY`
* `VITE_RAPIDAPI_KEY` (Subscribe to [Yahoo Finance API](https://rapidapi.com/rapidapi-org1-rapidapi-org-default/api/yahoo-finance187))

### 3. Development
* **Start dev server:** `npm run dev`
* **Build for production:** `npm run build`
* **Preview production build:** `npm run preview`

---

## Recommended Setup

### IDE
* **VS Code** + [**Vue (Official)**](https://marketplace.visualstudio.com/items?itemName=Vue.volar) extension.
* *Note: Disable Vetur to avoid conflicts.*

### Browser Extensions
To make development easier, install the **Vue.js devtools** for your browser and enable **Custom Object Formatters** in your DevTools settings.

---

## Technical Details
* **Framework:** Vue.js
* **Styling:** Bootstrap
* **Build Tool:** Vite
* **Linting:** ESLint