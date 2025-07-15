
# For Copilot/AI Agents

> **Note for Copilot/AI Agents and Contributors:**
>
> This project is designed to closely follow the official ForgeRock sample app ([reactjs-todo-davinci](https://github.com/ForgeRock/sdk-sample-apps/tree/main/javascript/reactjs-todo-davinci)).
>
> **When updating, extending, or debugging this app:**
> - **Reference the official sample app** for best practices, component logic, and orchestration patterns.
> - **Keep the following in sync:**
>   - Authentication flow logic (see `Form`, `useDavinci`, and related components)
>   - Routing and protected resource patterns
>   - E2E tests (see `e2e/` and Playwright usage)
>   - API/server logic (if present)
>   - Environment/configuration files
> - **Update this README** with any new troubleshooting, setup, or architectural notes.
> - **Checklist for Copilot/AI agents:**
>   1. Compare all new/changed code to the official sample app.
>   2. Ensure all dependencies and scripts in `package.json` are up to date.
>   3. Add/modify components in `client/components/davinci-client/` as needed.
>   4. Keep E2E tests in `e2e/` comprehensive and passing.
>   5. Document any deviations from the official sample app here.
>   6. If in doubt, ask the user for clarification or reference the official repo.

# Quick Start

```sh
git clone https://github.com/cprice-ping/PingSDK-DaVinci-React.git
cd PingSDK-DaVinci-React
npm install
npm start
# Visit http://localhost:8443
```

## Requirements

- **Node.js** >= 14 (recommended: latest LTS)
- **npm** >= 7

Check your versions:
```sh
node --version
npm --version
```
If you have issues, consider using [nvm](https://github.com/nvm-sh/nvm) to manage Node versions.

---

## Common Errors & FAQ

**Q: I get `Module not found: Error: Can't resolve 'sass-loader'` or `html-webpack-plugin`?**

A: Run:
```sh
npm install --save-dev sass-loader sass html-webpack-plugin
```

**Q: My .env changes aren't picked up?**

A: Restart the dev server after editing `.env`.

**Q: Port 8443 is in use?**

A: Change `PORT` in `.env` and update `REDIRECT_URI` accordingly.

**Q: API errors?**

A: Ensure `API_URL` is correct and your PingOne tenant is reachable.

---

# Example Component Implementations

## Sample package.json

Below is a sample `package.json` for this project. Use this as a reference for required dependencies and scripts:

```json
{
  "name": "pingsdk-davinci-react",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "start": "webpack serve --mode development --open --port 8443",
    "build": "webpack --mode production",
    "e2e": "playwright test",
    "e2e:ui": "playwright test --ui"
  },
  "dependencies": {
    "@forgerock/davinci-client": "latest",
    "@forgerock/javascript-sdk": "latest",
    "react": "^18.0.0",
    "react-dom": "^18.0.0",
    "react-router-dom": "^6.0.0"
  },
  "devDependencies": {
    "@babel/core": "^7.20.0",
    "@babel/preset-env": "^7.20.0",
    "@babel/preset-react": "^7.18.6",
    "@playwright/test": "^1.40.0",
    "babel-loader": "^9.1.0",
    "css-loader": "^6.7.3",
    "dotenv-webpack": "^8.0.1",
    "eslint": "^8.0.0",
    "html-webpack-plugin": "^5.6.3",
    "mini-css-extract-plugin": "^2.7.6",
    "prettier": "^3.0.0",
    "sass": "^1.62.0",
    "sass-loader": "^13.3.2",
    "style-loader": "^3.3.2",
    "webpack": "^5.75.0",
    "webpack-cli": "^5.0.1",
    "webpack-dev-server": "^4.11.1"
  }
}
```

Below are example implementations for several key components. Use these as a starting point if building from scratch.

---

### `Form` (Authentication Flow Container)
```jsx
import Alert from './alert';
import Password from './password';
# Official ForgeRock Sample Repo

For full, production-ready implementations and advanced usage, see the official ForgeRock sample app:

- https://github.com/ForgeRock/sdk-sample-apps/tree/main/javascript/reactjs-todo-davinci

This repo contains:
- Complete source code for all components and hooks
- Advanced DaVinci orchestration patterns
- Additional documentation and troubleshooting
export default function Form() {
  // Compose state and handlers for the DaVinci flow here
  // Render appropriate input components based on flow node
  return (
    <form>
      {/* Example usage: */}
      <Text collector={...} inputName="username" updater={...} />
      <Password collector={...} inputName="password" updater={...} />
      <Alert message="Error message" type="error" />
      {/* ...other dynamic fields and submit button */}
    </form>
  );
}
```

### `Alert`
```jsx
import React from 'react';
export default function Alert({ message, type }) {
  return (
    <div className={`alert alert-${type}`}>{message}</div>
  );
}
```

### `Password`
```jsx
import React, { useState } from 'react';
export default function Password({ collector, inputName, updater }) {
  const [isVisible, setVisibility] = useState(false);
  return (
    <div>
      <input
        type={isVisible ? 'text' : 'password'}
        name={inputName}
        onChange={e => updater(e.target.value)}
      />
      <button type="button" onClick={() => setVisibility(v => !v)}>
        {isVisible ? 'Hide' : 'Show'}
      </button>
    </div>
  );
}
```

### `Text`
```jsx
import React from 'react';
export default function Text({ collector, inputName, updater }) {
  return (
    <input
      type="text"
      name={inputName}
      onChange={e => updater(e.target.value)}
      placeholder={collector?.output?.label || inputName}
    />
  );
}
```

---

#
# Official ForgeRock Sample Repo

For full, production-ready implementations and advanced usage, see the official ForgeRock sample app:

- https://github.com/ForgeRock/sdk-sample-apps/tree/main/javascript/reactjs-todo-davinci

This repo contains:
- Complete source code for all components and hooks
- Advanced DaVinci orchestration patterns
- Additional documentation and troubleshooting

---
#
# Component Reference

Below are the key React components and hooks used for DaVinci orchestration and authentication flows. All are located under `client/components/davinci-client/` unless otherwise noted.

| Component/Hook         | Type         | Description |
|------------------------|--------------|-------------|
| `Form`                 | Component    | Main view for managing the user authentication journey. Dynamically renders DaVinci flow nodes and collects user input. |
| `Alert`                | Component    | Displays form errors or success messages. |
| `Error`                | Component    | Renders error messages from the flow. |
| `FlowLink`             | Component    | Renders a link to start a new DaVinci flow. |
| `Password`             | Component    | Renders a password input with show/hide toggle. |
| `Protect`              | Component    | Handles the "Protect" node in DaVinci flows (mocked for demo). |
| `Readonly`             | Component    | Displays read-only values from the flow. |
| `SingleSelect`         | Component    | Renders a dropdown for single-select options. |
| `SubmitButton`         | Component    | Renders a submit button with loading spinner. |
| `Text`                 | Component    | Renders a text input for user entry. |
| `ObjectValueComponent` | Component    | Handles device selection and phone number input. |
| `Unknown`              | Component    | Fallback for unknown collector types in the flow. |
| `useDavinci`           | Hook         | Custom React hook to manage DaVinci flow state and client. |
| `useOAuth`             | Hook         | Custom React hook to manage OAuth state and user info. |
| `createClient`         | Utility      | Helper to initialize a DaVinci client instance. |

Other folders like `icons/`, `layout/`, `todo/`, and `utilities/` provide supporting UI and logic components for the rest of the app.

---

# PingSDK + DaVinci React Sample App (Copilot-Optimized)

## Overview

This project demonstrates how to build a modern React app using the Ping Identity JavaScript SDK and DaVinci client for authentication and orchestration. It includes a REST API, E2E tests, and a clear structure for rapid prototyping or production use.

---

## Prerequisites

- **Node.js** >= 14 (recommended: latest LTS)
- **npm** >= 7
- A **PingOne tenant** with SSO and DaVinci enabled
- An **OIDC Web App** configured in PingOne

---

## Project Structure

```
uglybaby/
├── .env                  # Environment variables (not committed)
├── .gitignore            # Ignores .env and node_modules
├── package.json          # Project dependencies and scripts
├── webpack.config.js     # Webpack build config
├── babel.config.js       # Babel config for React
├── public/
│   └── index.html        # Main HTML template
├── client/
│   ├── index.js          # React app entry point
│   ├── components/       # React components (DaVinci, etc.)
│   └── ...
├── e2e/                  # Playwright E2E tests
├── ping-davinci-app/     # (Optional) Additional app modules
└── ...
```

---

## Dependencies

**Core:**
- react, react-dom, react-router-dom
- @forgerock/javascript-sdk
- @forgerock/davinci-client

**Build & Tooling:**
- webpack, webpack-cli, webpack-dev-server
- babel, babel-loader, @babel/preset-react, @babel/preset-env
- dotenv, mini-css-extract-plugin, sass, sass-loader, style-loader, css-loader, html-webpack-plugin

**API & Utilities:**
- express, pouchdb, cors, cookie-parser, uuid

**Testing:**
- @playwright/test

**Linting/Formatting:**
- eslint, prettier, related plugins

---

## Environment Variables

Create a `.env` file in the project root (never commit this file):

```env
API_URL=http://localhost:9443
DEBUGGER_OFF=true
DEVELOPMENT=true
PORT=8443
WEB_OAUTH_CLIENT=your-ping-client-id
REDIRECT_URI=http://localhost:8443
SCOPE="openid profile email phone name revoke"
WELLKNOWN_URL=https://your-tenant.oidc/.well-known/openid-configuration
```

---

## Setup & Run


1. **Install dependencies:**
   ```sh
   npm install
   # If you see errors about missing loaders or plugins, also run:
   npm install --save-dev sass-loader sass html-webpack-plugin
   ```

2. **Start the app:**
   ```sh
   npm start
   ```

3. **Open your browser:**
   Visit [http://localhost:8443](http://localhost:8443)

---

## DaVinci Integration Example

Minimal usage in a React component:

```javascript
import React, { useEffect, useRef } from 'react';
import { davinci } from '@forgerock/davinci-client';

function DaVinciFlow() {
  const containerRef = useRef(null);
  useEffect(() => {
    const config = {
      serverConfig: {
        clientId: process.env.WEB_OAUTH_CLIENT,
        wellKnownUrl: process.env.WELLKNOWN_URL,
        redirectUri: process.env.REDIRECT_URI,
        scope: process.env.SCOPE,
        debuggerOff: process.env.DEBUGGER_OFF === 'true',
      },
      target: containerRef.current,
    };
    const unmount = davinci(config);
    return () => unmount && unmount();
  }, []);
  return <div ref={containerRef} style={{ minHeight: 500 }} />;
}
export default DaVinciFlow;
```

---

## Authentication Flow

1. User clicks **Sign In**
2. DaVinci flow is rendered and handles authentication
3. On success, user is greeted and can access protected resources

---

## E2E Testing

- E2E tests are in the `e2e/` folder and use Playwright
- Run all tests:
  ```sh
  npm run e2e
  ```
- Run Playwright UI:
  ```sh
  npm run e2e:ui
  ```

---

## Troubleshooting & Customization

- **.env not working?** Make sure you restart the dev server after editing `.env`.
- **Port in use?** Change `PORT` in `.env` and update `REDIRECT_URI` accordingly.
- **API errors?** Ensure `API_URL` is correct and your PingOne tenant is reachable.
- **Styling:** Uses Bootstrap 5 and custom classes (`cstm_`). Edit `client/styles/` for custom styles.
- **Debugging SDK:** Set `DEBUGGER_OFF=false` in `.env` to enable SDK integration breakpoints.

---

## Learn More

- [Ping Identity Docs](https://docs.pingidentity.com/)
- [DaVinci Docs](https://docs.pingidentity.com/access/daVinci/)
- [React Docs](https://react.dev/)
- [Playwright Docs](https://playwright.dev/)

---

## License

MIT License. See LICENSE file for details.
