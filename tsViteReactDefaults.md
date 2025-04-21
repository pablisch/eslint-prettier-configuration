# Default Eslint settings in TypeScript Vite React project

**NOTE:** at time of writing, 21.04.2025.

Any TypeScript Vite React project has Eslint built-in but no prettier. The default Eslint config is:

`eslint.config.js`
```javascript
import js from '@eslint/js'
import globals from 'globals'
import reactHooks from 'eslint-plugin-react-hooks'
import reactRefresh from 'eslint-plugin-react-refresh'
import tseslint from 'typescript-eslint'

export default tseslint.config(
  { ignores: ['dist'] },
  {
    extends: [js.configs.recommended, ...tseslint.configs.recommended],
    files: ['**/*.{ts,tsx}'],
    languageOptions: {
      ecmaVersion: 2020,
      globals: globals.browser,
    },
    plugins: {
      'react-hooks': reactHooks,
      'react-refresh': reactRefresh,
    },
    rules: {
      ...reactHooks.configs.recommended.rules,
      'react-refresh/only-export-components': [
        'warn',
        { allowConstantExport: true },
      ],
    },
  },
)
```
And there are the following scripts and dependencies in `package.json`:

```json
{
  "name": "portfolio-ts",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "start": "vite",
    "build": "tsc -b && vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "prettier": "prettier . --write",
    "format": "npm run lint && npm run prettier",
    "test": "vitest --globals --reporter verbose"
  },
  "dependencies": {
    "@testing-library/user-event": "^14.5.2",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router": "^6.25.1",
    "react-router-dom": "^6.25.1"
  },
  "devDependencies": {
    "@testing-library/dom": "^10.4.0",
    "@testing-library/jest-dom": "^6.6.2",
    "@testing-library/react": "^16.0.1",
    "@types/react": "^18.3.3",
    "@types/react-dom": "^18.3.0",
    "@types/testing-library__jest-dom": "^6.0.0",
    "@typescript-eslint/eslint-plugin": "^7.15.0",
    "@typescript-eslint/parser": "^7.15.0",
    "@vitejs/plugin-react": "^4.3.1",
    "@vitejs/plugin-react-swc": "^3.5.0",
    "eslint": "^8.57.0",
    "eslint-plugin-react-hooks": "^4.6.2",
    "eslint-plugin-react-refresh": "^0.4.7",
    "jsdom": "^24.1.1",
    "prettier": "3.3.3",
    "typescript": "^5.2.2",
    "vite": "^5.3.4",
    "vitest": "^2.0.4"
  }
}
```