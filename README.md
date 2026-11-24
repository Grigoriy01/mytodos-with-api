# ✅ Todo App with REST API

A responsive, production-ready task management application built with **React**, **TypeScript**, and **SCSS**. Integrated with a remote REST API for real-time data synchronization and full CRUD functionality.

🔗 **[Live Demo](https://grigoriy01.github.io/mytodos-with-api/)**

---

## ✨ Features

- **Full CRUD Operations:** Create, read, toggle completion status, inline-edit titles, and delete todos via REST API.
- **Smart Filtering:** Filter tasks by status (`All`, `Active`, `Completed`) with instant UI updates and active items counter.
- **Bulk Actions:** Toggle all tasks at once or clear all completed items in a single click.
- **Optimistic & Loader States:** Visual feedback (spinners/disabled states) per item during asynchronous network requests.
- **Error Handling:** Graceful error notifications with auto-dismiss and manual close options.
- **Keyboard Accessibility:** Full keyboard support (`Enter` to submit/save, `Escape` to cancel inline editing).

---

## 🛠 Tech Stack

- **Core:** React 18, TypeScript
- **Build Tool:** Vite
- **Styling:** SCSS (BEM methodology, modular architecture)
- **Networking:** Fetch API / RESTful architecture
- **Code Quality:** ESLint, Prettier

---

## 🏗 Architecture & Key Decisions

- **Modular Structure:** Organized the project into clear, separate folders for UI components, API services, TypeScript types, and helper utilities to keep the code clean and easy to maintain.
- **API & Per-Item Loading States:** Moved all HTTP requests into a dedicated API layer and tracked active network operations by item ID (`processingIds`). This ensures that only the specific todo being updated or deleted shows a loading spinner, rather than blocking the entire list.
**Optimistic UI with Temporary Placeholder (`tempTodo`):** When creating a new task, the app immediately renders a temporary placeholder item (`tempTodo`) with a loading state before the server responds. Once the POST request succeeds, the placeholder is seamlessly replaced with the real todo from the API (or removed if the request fails), avoiding awkward UI delays or empty jumps.
- **Edge-Case & Error Handling:** Added input trimming to prevent empty tasks, keyboard shortcuts (`Enter` to save, `Escape` to cancel inline editing), and an auto-dismissing notification banner to clearly inform the user if a network request fails.
- **State & Performance:** Kept the single source of truth in the main component and used `useMemo` to filter tasks (`All`, `Active`, `Completed`) and calculate counters without unnecessary recalculations.

---

## 🚀 Getting Started

### Prerequisites
- Node.js (v18+ recommended)
- npm

### Installation & Local Development

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/Grigoriy01/mytodos-with-api.git](https://github.com/Grigoriy01/mytodos-with-api.git)
   cd mytodos-with-api
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Run the development server:**
   ```bash
   npm run dev
   ```

4. **Build for production:**
   ```bash
   npm run build
   ```
