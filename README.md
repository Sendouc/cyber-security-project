For university course `Cyber Security Base 2020`. Modified from https://github.com/prisma/prisma-examples/blob/latest/typescript/rest-nextjs-api-routes

⚠️ Intentionally contains security flaws ⚠️

## How to use

_Make sure you have Git and Node installed_

### 1. Download example & install dependencies

Download this example:

```
curl https://codeload.github.com/prisma/prisma-examples/tar.gz/latest | tar -xz --strip=2 prisma-examples-latest/typescript/rest-nextjs-api-routes
```

Install npm dependencies:

```
cd rest-nextjs-api-routes
npm install
```

Note that this also generates Prisma Client JS into `node_modules/@prisma/client` via a `postinstall` hook of the `@prisma/client` package from your `package.json`.

<Details><Summary><strong>Alternative (without curl):</strong> Clone the entire repo</Summary>

Clone this repository:

```
git clone git@github.com:prisma/prisma-examples.git --depth=1
```

Install npm dependencies:

```
cd prisma-examples/typescript/rest-nextjs-api-routes
npm install
```

</Details>

### 2. Start the app

```
npm run dev
```

The app is now running, navigate to [`http://localhost:3000/`](http://localhost:3000/) in your browser to explore its UI.
