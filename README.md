# PingSDK + DaVinci React Sample App

## Overview

This project demonstrates how to use the Ping Identity JavaScript SDK and DaVinci client in a React app for authentication and orchestration.

## Prerequisites

- Node.js >= 14
- A PingOne tenant with SSO and DaVinci enabled
- OIDC Web App configured in PingOne

## Environment Variables

Create a `.env` file in the project root with:

```
API_URL=http://localhost:9443
DEBUGGER_OFF=true
DEVELOPMENT=true
PORT=8443
CLIENT_ID=your-ping-client-id
REDIRECT_URI=http://localhost:8443
SCOPE="openid profile email phone name revoke"
WELLKNOWN_URL=https://your-tenant.oidc/.well-known/openid-configuration
```

## Setup

1. Install dependencies:
   ```
   npm install
   ```

2. Start the app:
   ```
   npm start
   ```

3. Visit [http://localhost:8443](http://localhost:8443) in your browser.

## Minimal DaVinci Integration Example

In your main React component:

```javascript
import React, { useEffect, useRef } from 'react';
import { davinci } from '@forgerock/davinci-client';

function App() {
  const containerRef = useRef(null);

  useEffect(() => {
    const config = {
      serverConfig: {
        clientId: process.env.CLIENT_ID,
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

export default App;
```

## Authentication Flow

- User clicks "Sign In"
- DaVinci flow is rendered and handles authentication
- On success, user is greeted and can access protected resources

## Testing

- E2E tests are in the `e2e/` folder and use Playwright.
- Run tests with:
  ```
  npm run e2e
  ```
