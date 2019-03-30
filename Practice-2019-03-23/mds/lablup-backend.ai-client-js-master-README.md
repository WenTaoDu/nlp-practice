# Backend.AI Client for Javascript (node.js / ES6+)

## Requirements

This client SDK runs on CommonJs (with node.js) / ES6-compatible Javascript runtimes with async/await supports
such as NodeJS 7+ and modern web browsers released since 2017.

This client library supports Backend.AI API v2/3.

## Install

```console
$ npm install backend.ai-client
```
You can also use `yarn`.

```console
$ yarn install backend.ai-client
```

## Build

```
$ npm install -g browserify
$ make es6
```

## Usage

TypeScript:
```typescript
import * as ai from 'backend.ai-client-node';

let config = ai.backend.ClientConfig.createFromEnv();
let client = new ai.backend.Client(config);
```

CommonJS-style:
```javascript
const ai = require('backend.ai-client-node');

let config = ai.backend.ClientConfig.createFromEnv();
let client = new ai.backend.Client(config);
```

ES6:
```
import './backend.ai-client-es6.js';

let config = new ai.backend.ClientConfig(
  '[ADD_ACCESS_KEY_HERE]',
  '[ADD_SECRET_KEY_HERE]',
  '[ENDPOINT_HERE]'
);
let client = new ai.backend.Client(
  config,
  `Backend.AI ES6 App.`,
);
```

When creating `ClientConfig` object, you can manually pass `accessKey`,
`secretKey`, and optional `endpoint` arguments.
The environment variables are:
* `BACKEND_ACCESS_KEY`
* `BACKEND_SECRET_KEY`
* `BACKEND_ENDPOINT` (optional, defaults to `https://api.backend.ai`)

All API functions return a promise that resolves into a parsed object
when success according to server-provided `Content-Type` and rejects with an
object with `type` and `message` attributes if failed.

```javascript
client.createIfNotExists('python:latest', 'my-session-id')
.then(response => {
  console.log(`my session is created: ${response.kernelId}`);
}).catch(err => {
  switch (err.type) {
  case ai.backend.Client.ERR_SERVER:
    console.log(`session creation failed: ${err.message}`);
    break;
  default:
    console.log(`request/response failed: ${err.message}`);
  }
});
```

The result objects returned with success has different formats API by API.
Please check out [our official documentation](https://www.backend.ai/#/docs/index).

`err.type` is one of the following values:

* `ai.backend.Client.ERR_SERVER`: The server responded with failure.
  In this case, `err.message` includes HTTP status and additional error information
  returned by the API server.
* `ai.backend.Client.ERR_RESPONSE`: An error occurred while reading the response.
  `err.message` includes an exception value passed from your Javascript runtime.
* `ai.backend.Client.ERR_REQUEST`: An error occurred while sending the request.
  `err.message` includes an exception value passed from your Javascript runtime.

