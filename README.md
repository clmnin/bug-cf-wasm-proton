# Bug: Can't run cf-wasm/proton in opennext-cloudflare

I added [@cf-wasm/proton](https://github.com/fineshopdesign/cf-wasm/tree/main/packages/photon) and tried running the example api route to resize an image using [photon](https://github.com/silvia-odwyer/photon) (A high-performance image processing library written in rust). 

While it works in the next js environment, it does not work in opennext-cloudflare's workerd environment

> [!CAUTION]
> Middleware is not supported in minimal mode. Please remove the `NEXT_MINIMAL` environment variable

## next dev
```sh
# "dev": "next dev"
npm run dev
```
Logs:
```log
> bug-cf-wasm-proton@0.1.0 dev
> next dev

  ▲ Next.js 14.2.5
  - Local:        http://localhost:3000

 ✓ Starting...
 ✓ Ready in 1663ms
 ✓ Compiled /api/image in 141ms (137 modules)
 GET /api/image 200 in 236m
```
## workerd
To run in [workerd](https://github.com/cloudflare/workerd)
```sh
# "preview": "cloudflare && wrangler dev"
npm run preview
```

```log
> bug-cf-wasm-proton@0.1.0 preview
> cloudflare && wrangler dev

Building the Next.js app in the current folder (/Users/bug-report/bug-cf-wasm-proton)
  ▲ Next.js 14.2.5

   Creating an optimized production build ...
 ✓ Compiled successfully
 ✓ Linting and checking validity of types    
 ⚠ Using edge runtime on a page currently disables static generation for that page
 ✓ Collecting page data    
 ✓ Generating static pages (5/5)
 ✓ Collecting build traces    
 ✓ Finalizing page optimization    

Route (app)                              Size     First Load JS
┌ ○ /                                    5.25 kB        92.3 kB
├ ○ /_not-found                          871 B          87.9 kB
└ ƒ /api/image                           0 B                0 B
+ First Load JS shared by all            87 kB
  ├ chunks/23-b75664ace61c0abb.js        31.5 kB
  ├ chunks/fd9d1056-2821b0f0cabcd8bd.js  53.6 kB
  └ other shared chunks (total)          1.86 kB


○  (Static)   prerendered as static content
ƒ  (Dynamic)  server-rendered on demand

⚙️ Copying files...

# copyPrerenderedRoutes
# copyPackageTemplateFiles
⚙️ Bundling the worker file...

# patchWranglerDeps
# updateWebpackChunksFile
 - chunk 347.js
 - chunk 682.js
 - chunk 948.js
# patchRequire
# patchReadFile
# inlineNextRequire
# patchFindDir
# inlineEvalManifest
# patchCache
# inlineMiddlewareManifestRequire
# patchExceptionBubbling
Worker saved in `/Users/bug-report/bug-cf-wasm-proton/.worker-next/index.mjs` 🚀


 ⛅️ wrangler 3.84.1
-------------------

[wrangler:inf] Ready on http://localhost:8787
⎔ Starting local server...
✘ [ERROR] ⨯ Error: Middleware is not supported in minimal mode. Please remove the `NEXT_MINIMAL` environment variable.

      at Object.runEdgeFunction
  (file:///Users/bug-report/bug-cf-wasm-proton/.worker-next/index.mjs:31337:17)
      at Object.handleCatchallRenderRequest
  (file:///Users/bug-report/bug-cf-wasm-proton/.worker-next/index.mjs:30432:42)
      at async Object.runImpl
  (file:///Users/bug-report/bug-cf-wasm-proton/.worker-next/index.mjs:23016:9)
      at async Object.handleRequestImpl
  (file:///Users/bug-report/bug-cf-wasm-proton/.worker-next/index.mjs:22948:18)


[wrangler:inf] GET /api/image 500 Internal Server Error (95ms)
```