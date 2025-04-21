# Eslint and Prettier Configuration

This README is concerned with Eslint and Prettier configuration used in TypeScript Vite React projects. Links to other project configurations will be added as needed.

**NOTE:** The current versions, at time of writing, are `eslint` v.9.25.0 and `prettier` v.3.5.3.

## Configuration for TypeScript Vite React project

### The TypeScript Vite React defaults

See [defaults page](https://github.com/pablisch/eslint-prettier-configuration/blob/main/tsViteReactDefaults.md)

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
"lint": "eslint . --ext ts,tsx --report-unused-disable-directives --fix",
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


