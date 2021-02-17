# Example of Parcel 2 failing with Web Workers inside of a split module

To try this out, copy this into your `parcel/` repository at `packages/examples/`, `cd` into the directory, and run:

```
yarn run parcel build --no-scope-hoist --no-cache index.html
```

You will get output similar to this:
```
dist/browser/index.fe0b0221.js                            70.25 KB    2.57s                     
├── ../../../node_modules/lodash/lodash.js                69.08 KB    417ms                     
└── Code from unknown sourcefiles                          1.17 KB      0ms                     

dist/browser/index.85b8c16e.js                              3.8 KB    974ms                     
├── Code from unknown sourcefiles                          1.29 KB      0ms                     
├── ../../runtimes/js/src/JSRuntime.js                       625 B     22ms                     
├── ../../runtimes/js/src/relative-path.js                   478 B     49ms                     
├── ../../runtimes/js/src/bundle-url.js                      447 B     50ms                     
├── ../../runtimes/js/src/loaders/browser/js-loader.js       407 B     45ms                     
├── ../../runtimes/js/src/cacheLoader.js                     282 B      9ms                     
├── ../../runtimes/js/src/bundle-manifest.js                 246 B     39ms                     
└── src/index.js                                              85 B    1.95s                     

dist/browser/split.0112a5d5.js                             1.61 KB    973ms                     
├── Code from unknown sourcefiles                          1.14 KB      0ms                     
├── ../../runtimes/js/src/get-worker-url.js                  221 B     51ms                     
├── ../../runtimes/js/src/JSRuntime.js                       157 B     23ms                     
└── src/split.js                                             105 B     36ms                     
# ^^ LODASH NOT INCLUDED

dist/browser/worker.f5b2ae32.js                            1.38 KB    974ms                     
└── src/worker.js                                             19 B    378ms                     
# ^^ LODASH NOT INCLUDED

dist/browser/index.html                                      223 B    972ms                     
└── index.html                                               228 B    1.82s                     
Done in 8.79s.  
```

If you look in the `worker.[hash].js` and `split.[hash].js` files, they point
to `lodash` but don't actually include it, triggering an error `Cannot find
module` on page load.
