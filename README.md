# Eslint and Prettier Configuration

This README is concerned with Eslint and Prettier configuration used in TypeScript Vite React projects. Links to other project configurations will be added as needed.

## Configuration for TypeScript Vite React project

### The TypeScript Vite React defaults

**NOTE:** The current versions, at time of writing, are `eslint` v.9.25.0 and `prettier` v.3.5.3.

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

### Install dependencies

Save exact versions of eslint and prettier:

```bash
npm install --save-dev --save-exact eslint prettier
```

Save other dependencies:

```bash
npm install --save-dev \
eslint-config-prettier \
eslint-plugin-prettier \
eslint-plugin-react \
eslint-plugin-react-hooks \
@typescript-eslint/eslint-plugin \
@typescript-eslint/parser \
@stylistic/eslint-plugin \
eslint-plugin-react-refresh
```

**NOTE:** The last two are questionable, considered niche and worth looking into at some point. They are not fundamentally needed but are used in my current configuration, which may not be optimal:

- `@stylistic/eslint-plugin` - This is a fork of ESLint's stylistic rules – may be useful but overlaps with Prettier.
- `eslint-plugin-react-refresh` - Only for hot reloading dev scenarios – usually not necessary for linting.

### Add scripts

Add scripts to `package.json`:

```bash
"lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
"prettier": "prettier . --write",
"format": "npm run lint && npm run prettier",
```

**NOTE:** `format` is something that I previously added as `pretty` and there is a `zshrc` alias for both `pretty` and `format` to run scripts for `lint` and `prettier` together.

### Add prettier config

Add a `.prettierrc` and `.prettierignore` file: `touch .prettierrc .prettierignore`

Add files and directories to ignore in `.prettierignore`:

```bash
node_modules
.env
```

My current default prettier config is:

```json
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "printWidth": 80,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "singleAttributePerLine": false,
  "jsxSingleQuote": false,
  "bracketSameLine": false,
  "arrowParens": "avoid",
  "endOfLine": "auto"
}
```

### Update Eslint config

My current Eslint config:

```bash
import js from '@eslint/js'
import globals from 'globals'
import reactHooks from 'eslint-plugin-react-hooks'
import reactRefresh from 'eslint-plugin-react-refresh'
import tseslint from 'typescript-eslint'
import prettier from 'eslint-plugin-prettier'
import react from 'eslint-plugin-react'
import prettierConfig from 'eslint-config-prettier'
import stylistic from '@stylistic/eslint-plugin'

export default tseslint.config(
  {
    ignores: ['dist'],
  },
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      js.configs.recommended,
      ...tseslint.configs.recommended,
      prettierConfig,
    ],
    languageOptions: {
      ecmaVersion: 2020,
      globals: {
        ...globals.browser,
      },
      parser: tseslint.parser,
      parserOptions: {
        ecmaFeatures: {
          jsx: true,
        },
      },
    },
    plugins: {
      'react-hooks': reactHooks,
      'react-refresh': reactRefresh,
      prettier: prettier,
      react: react,
      '@stylistic': stylistic,
    },
    rules: {
      ...reactHooks.configs.recommended.rules,
      '@stylistic/padding-line-between-statements': [
        'error',
        { blankLine: 'always', prev: 'function', next: 'return' },
        { blankLine: 'always', prev: '*', next: 'return' },
      ],
      'no-extra-semi': 'off',
      'lines-between-class-members': 'off',
      'padding-line-between-statements': 'off',
      'prettier/prettier': [
        'error',
        {
          usePrettierrc: true,
        },
      ],
    },
  }
)

```


