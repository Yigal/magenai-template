# MagenAI Project - Summary Guidelines

## Project Overview
A Chrome Extension (v1.2) that protects sensitive data from being sent to AI models like ChatGPT.

---

## Architecture & Technology Stack
**Tech**: React 19, TypeScript 5.8.2, Vite 6, Chrome Manifest V3, Lucide React icons.

---

## Extension Configuration
**Target**: ChatGPT (chatgpt.com) - includes service worker, content scripts, side panel UI, and CSS resources.

---

## Installation & Development
**Setup**: Install Node.js, run `npm install`, set GEMINI_API_KEY in .env.local, then `npm run dev` on port 3000.

---

## Build & Deployment
**Commands**: `npm run dev` (development), `npm run build` (production), `npm run preview` (local preview).

---

## TypeScript Settings
**Target**: ES2022 with JSX support, path aliases (@/*), and module bundler resolution for imports.

---

## Vite Configuration
**Dev Server**: Port 3000 on 0.0.0.0, environment variables loaded from .env.local, React hot reload enabled.

---

## Tab Switching Error (Common Issue)
**Problem**: Missing `renderStore` and `renderDashboard` functions in popup.js cause immediate crash.
**Fix**: Restore deleted functions and ensure they're called in DOMContentLoaded event listener.

---

## Debugging Chrome Extensions
**Method**: Right-click extension → "Inspect Popup", check Console for red errors, verify HTML IDs match JavaScript references.

---

## Code Organization
**Pattern**: Keep initialization in DOMContentLoaded, define all functions at module level, separate UI rendering from state management.

---

## Security Best Practices
**Key Points**: Store API keys in .env.local only, restrict host permissions to chatgpt.com, keep JavaScript isolated from external pages.

---

## React & UI Components
**Framework**: React 19.2.3 with TypeScript, Lucide React icons, functional components with hooks.

---

## Deployment Process
**Steps**: Run `npm run build`, package dist/ folder, update manifest version, submit to Chrome Web Store.

---

## Testing & QA
**Approach**: Manual testing in DevTools, console inspection, network monitoring, verify HTML-JavaScript element matching.

---

## Documentation Standards
**Location**: chrome_app/docs/ - include root cause, prevention tips, and step-by-step solutions for errors.
