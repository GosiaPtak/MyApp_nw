# Running this NW.js app on macOS (Apple Silicon)

## Problem summary

If you try to run this project using:

```bash
yarn nw .
```

you may see an error like:

```
Cannot open .../nwjs.app/Contents/Resources/app.nw
FILE_ERROR_NOT_FOUND
```

and a window that:

* opens but appears frozen
* cannot be resized
* cannot be closed

This is **not a problem with the app code**.

---

## Why this happens

This project hits a **known NW.js ecosystem issue** when the following conditions are combined:

* macOS
* Apple Silicon (arm64)
* NW.js version **0.105+**
* Running the app via the **`nw` npm wrapper** (`yarn nw .`)

Under these conditions, the `nw` npm package often:

* downloads the NW.js runtime correctly
* **fails to create / link the `app.nw` bundle**

When this happens, NW.js launches without your application loaded, which leads to the error and the unresponsive window.

This is a tooling issue, not a repository or configuration error.

---

## Correct way to run the app (recommended)

**Do not use:**

```bash
yarn nw .
```

Instead, run the NW.js binary directly and point it at the project directory:

```bash
./node_modules/nw/nwjs-v0.106.1-osx-arm64/nwjs.app/Contents/MacOS/nwjs .
```

If you are using a different NW.js version, adjust the path accordingly.

This approach:

* bypasses the broken npm wrapper
* loads `index.html` correctly
* creates the app context properly

---

## Clean install (important)

If you previously installed a different NW.js version, do a clean reinstall:

```bash
rm -rf node_modules yarn.lock
yarn install
```

Then verify that only the expected NW.js version exists inside `node_modules/nw`.
