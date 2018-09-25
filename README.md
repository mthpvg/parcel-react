# parcel-react

## Warning
We are currently using `parcel-bundler@1.10.0-beta.1` to be able to benefit from `babel` 7 new release.

## Install
```bash
git clone git@github.com:mthpvg/parcel-react.git
cd parcel-react
npm install
```

## Development
```bash
npm start
```
Visit: http://localhost:1234

## Production

### Build
```bash
npm run build
```
Output is in the `dist` directory.

### Serve build locally
```bash
npm run serve-build
```
Visit: http://localhost:5000

## Details

### Webmanifest
Parcel is not able to pick `manifest.json`, instead it parses:
```html
<link rel="manifest" href="manifest.webmanifest">
```

### Service Worker
The `dist/service-worker.js` file is generated by the Workbox library based on the `workbox-config.js` config file. In order to generate the list of files to cache, first workbox needs parcel to produce those files.

We have a chicken and egg problem because parcel picks up the service worker file by looking its registration in the source code:
https://github.com/parcel-bundler/parcel/pull/398

To solve this problem we give parcel an empty service worker file: `src/service-worker.js`. When running `npm run build`, parcel copies that file to the `dist` directory then the post hook `postbuild` is triggered and workbox replaces the empty service worker with the real one.
