# MagenAI Project - Full Guidelines

## Overview
MagenAI is a Chrome Extension (v1.2) designed to protect sensitive data from being sent to AI models, specifically ChatGPT. The extension intercepts requests and prevents sensitive information leakage.

---

## 1. Project Architecture

### Structure
- **Frontend**: React 19 with TypeScript
- **Build Tool**: Vite 6 with React plugin
- **Runtime**: Chrome Extension Manifest V3
- **Language**: TypeScript, JavaScript, and React

### Key Technologies
- React DOM for UI rendering
- Lucide React for UI icons
- TypeScript 5.8.2 for type safety
- Vite for fast development and production builds

### Directory Layout
```
chrome_app/
├── src/                 # React source files
│   └── contentScript.tsx
├── index.tsx           # React entry point
├── App.tsx             # Main React component
├── popup.js            # Popup script logic
├── background.js       # Service worker
├── content.js          # Content script
├── manifest.json       # Extension configuration
├── package.json        # Dependencies
├── tsconfig.json       # TypeScript config
├── vite.config.ts      # Vite configuration
├── docs/               # Documentation
│   ├── switch_tabs_error.md
│   └── switch_tabs_error_sum.md
└── metadata.json       # Extension metadata
```

---

## 2. Chrome Extension Configuration

### Manifest Details
- **Version**: 3 (latest Chrome standard)
- **Permissions**: activeTab, tabs, scripting, storage, sidePanel
- **Host Permissions**: Specifically targets `*://chatgpt.com/*`
- **Side Panel**: Default UI shown in index.html
- **Action**: Custom title "Open MagenAI"

### Key Components
1. **Background Service Worker**: `background.js` - Handles background tasks and messaging
2. **Content Scripts**: `content.js` - Runs on chatgpt.com pages to intercept data
3. **Side Panel UI**: React-based interface in `index.html`
4. **Web Accessible Resources**: `modal.css` accessible on ChatGPT domain

### Execution Flow
- Content script runs at `document_idle` on ChatGPT pages
- Service worker manages extension state and inter-component communication
- Side panel provides user interface for managing settings

---

## 3. Development Setup

### Prerequisites
- Node.js (required)
- npm (comes with Node.js)

### Installation Steps
1. Install dependencies: `npm install`
2. Configure API key: Set `GEMINI_API_KEY` in `.env.local`
3. Start development: `npm run dev` (runs on port 3000)

### Available Scripts
- `npm run dev` - Start Vite development server (port 3000, host 0.0.0.0)
- `npm run build` - Create production build
- `npm run preview` - Preview production build locally

### Environment Variables
- `GEMINI_API_KEY` - Required for API functionality
- Exposed to frontend as `process.env.GEMINI_API_KEY` and `process.env.API_KEY`

---

## 4. TypeScript Configuration

### Compiler Options
- **Target**: ES2022 (modern JavaScript features)
- **Module System**: ESNext with bundler resolution
- **JSX Support**: React JSX transform
- **Strict Mode**: Isolated modules enabled
- **Experimental**: Decorators allowed

### Path Aliases
- `@/*` maps to root directory for clean imports

### Key Settings
- Skip library type checking for faster compilation
- Allow importing TS extensions
- No emit (relies on Vite for compilation)
- Force module detection for proper ESM handling

---

## 5. Build Configuration (Vite)

### Development Server
- **Port**: 3000
- **Host**: 0.0.0.0 (accessible from any network interface)
- **Hot Reload**: Enabled with React plugin

### Production Build
- Optimized bundle generation
- Tree-shaking for unused code removal
- Code splitting for efficient loading

### Environment Handling
- Loads `.env.local` for local development
- Exposes `GEMINI_API_KEY` to application code
- Different configurations for dev and production modes

---

## 6. Common Issues and Troubleshooting

### Tab Switching Error

**Problem**: `Uncaught ReferenceError: renderStore is not defined`

**Root Cause**: Functions `renderStore` and `renderDashboard` were accidentally deleted from `popup.js`, likely during a Find/Replace operation.

**Location**: `popup.js` line 108

**Impact**: Extension crashes immediately upon opening because essential initialization functions are missing.

**Prevention Steps**:
1. Only replace the specific function intended, not entire file content
2. Verify function definitions exist before calling them
3. Keep a backup before large Find/Replace operations
4. Use version control to track changes

### Debugging Chrome Extension

**Steps**:
1. Right-click extension icon → "Inspect Popup"
2. Check Console tab for red errors (ReferenceError, TypeError, etc.)
3. Ensure all function definitions are present in the file
4. Verify HTML element IDs match JavaScript references
5. Check that event listeners are inside `DOMContentLoaded` block

### HTML-JavaScript Mismatch

**Problem**: JavaScript references undefined HTML elements

**Solution**:
- Verify every ID used in JavaScript (e.g., `tab-snippets`, `view-snippets`) exists in `index.html`
- Ensure IDs are spelled consistently
- Check that HTML elements are loaded before JavaScript executes

---

## 7. Code Organization Best Practices

### Function Restoration Pattern
```javascript
// After DOMContentLoaded
document.addEventListener('DOMContentLoaded', () => {
    // Initialize essential functions
    renderStore();
    renderDashboard();
    setupTabSwitching();
});

// Define all functions at module level
function setupTabSwitching() { /* ... */ }
function loadAppConfig() { /* ... */ }
function saveAppConfig() { /* ... */ }
function renderStore() { /* ... */ }
function renderDashboard() { /* ... */ }
```

### Module Structure
- Keep initialization logic in `DOMContentLoaded` callback
- Define helper functions at module scope
- Avoid nesting large functions inside event handlers
- Maintain clear separation between UI rendering and state management

---

## 8. Security Considerations

### Data Interception
- Extension has explicit host permission for `chatgpt.com` only
- Content scripts run in isolated context
- Side panel provides isolated UI context

### API Key Management
- Store `GEMINI_API_KEY` in `.env.local` (never commit to version control)
- Environment variables are bundled at build time
- Use Chrome's storage API for sensitive runtime data

### Content Security
- `modal.css` is the only accessible resource to external pages
- JavaScript content remains extension-isolated
- Cross-origin requests are restricted by manifest permissions

---

## 9. React and Component Architecture

### Framework Usage
- React 19.2.3 for UI components
- TypeScript for type safety
- Functional components with hooks

### UI Library
- Lucide React (v0.562.0) for consistent iconography

### Entry Points
- `index.tsx` - React application entry point
- `App.tsx` - Main component container
- `contentScript.tsx` - Content script React binding

---

## 10. Deployment and Distribution

### Build Process
1. Run `npm run build`
2. Creates optimized bundle in `dist/` directory
3. Vite handles all module resolution and bundling

### Extension Distribution
- Package contents of build directory
- Submit to Chrome Web Store
- Update version in `manifest.json` for new releases
- Maintain changelog for user-facing updates

### Version Management
- Current version: 1.2
- Update in `manifest.json` for releases
- Follow semantic versioning

---

## 11. Testing and Quality Assurance

### Testing Approach
- Manual testing in development mode
- Browser DevTools for debugging
- Console inspection for errors
- Network tab for API call monitoring

### Quality Checks
- Verify all HTML element IDs match JavaScript
- Test function availability before calling
- Check event listener attachment timing
- Validate environment variables are loaded

---

## 12. Documentation Standards

### Error Documentation
- Location: `chrome_app/docs/`
- Format: Markdown with code examples
- Include root cause analysis
- Provide prevention guidelines
- Add step-by-step solutions

### Code Comments
- Document non-obvious logic
- Explain why, not just what
- Keep comments up-to-date with code changes
