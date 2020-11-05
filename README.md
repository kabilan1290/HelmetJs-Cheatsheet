# HelmetJs-Cheatsheet

npm install helmet - To install helmet js

const helmet = require('helmet');  - 

app.use(helmet()) - To set headers based on best practices

app.use(helmet.hidePoweredBy()) - To remove X-Powered-By Header

app.use(helmet.hidePoweredBy({ setTo: 'KABI 1.2' })) - You can also explicitly set the header to something else, to throw people off.

app.use(helmet.frameguard({action: 'deny'})) - Set X-Frame-Options to deny,To prevent from clickjacking.we can also use other actions like same origin,allow-from.

app.use(helmet.xssFilter()) - It will set the Xss Protection header.

app.use(helmet.noSniff()) - It will set the X-Content-type-options to nosniff,This instructs the browser to not bypass the provided Content-Type.

app.use(helmet.ieNoOpen()) -Set the header x-download-options: noopen
, This will prevent IE users from executing downloads in the trusted site’s context.

// Sets "Strict-Transport-Security: max-age=123456; includeSubDomains"
app.use(
  helmet.hsts({
    maxAge: 123456,
  })
);

// Sets "Strict-Transport-Security: max-age=123456"
app.use(
  helmet.hsts({
    maxAge: 123456,
    includeSubDomains: false,
  })
);

// Sets "Strict-Transport-Security: max-age=123456; includeSubDomains; preload"
app.use(
  helmet.hsts({
    maxAge: 63072000,
    preload: true,
  })
);

app.use(helmet.dnsPrefetchControl()) - It set x-dns-prefetch-control: off
,To prevent DNS prefetching.

app.use(helmet.noCache()) - To prevent caching on client’s browser. 

// Sets "Content-Security-Policy: default-src 'self';script-src 'self' example.com;object-src 'none';upgrade-insecure-requests"
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", "example.com"],
      objectSrc: ["'none'"],
      upgradeInsecureRequests: [],
    },
  })
);

// Sets "Content-Security-Policy: default-src 'self';script-src 'self' example.com;object-src 'none'"
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      "default-src": ["'self'"],
      "script-src": ["'self'", "example.com"],
      "object-src": ["'none'"],
    },
  })
);

// Sets all of the defaults, but overrides script-src
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      ...helmet.contentSecurityPolicy.getDefaultDirectives(),
      "script-src": ["'self'", "example.com"],
    },
  })
);

// Sets the "Content-Security-Policy-Report-Only" header instead
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      /* ... */
    },
    reportOnly: true,
  })
);

// Sets "Content-Security-Policy: default-src 'self';script-src 'self' 'nonce-e33ccde670f149c1789b1e1e113b0916'"
app.use((req, res, next) => {
  res.locals.cspNonce = crypto.randomBytes(16).toString("hex");
  next();
});
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", (req, res) => `'nonce-${res.locals.cspNonce}'`],
    },
  })
);

