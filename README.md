# Image and PDF Compressor

A small local web application for resizing document photos, resizing signatures, cropping images, preparing images for a `75 KB to 100 KB` upload limit, and checking PDF compression.

The app runs in your browser. Image processing happens locally on your machine.

## Quick Start

From the `Resize` folder, run:

```bash
npm start
```

Then open:

```text
http://127.0.0.1:3000/
```

If port `3000` is already in use:

```bash
PORT=3001 npm start
```

Then open:

```text
http://127.0.0.1:3001/
```

## Open Without Server

You can also open the HTML file directly:

```text
file:///Users/schadha/Desktop/Resize/index.html
```

Running with `npm start` is recommended because it gives you a normal local web-app URL.

## What This App Does

- Accepts JPEG and PNG image uploads.
- Resizes photographs to exact document dimensions.
- Resizes signatures to exact signature dimensions.
- Lets you crop an image before resizing.
- Keeps aspect ratio when requested.
- Compresses images to the required file-size range.
- Accepts PDF uploads and checks/optimizes them for a `75 KB to 100 KB` upload requirement.
- Provides a preview, output dimensions, file size, and download button.

## Modes

### Photograph

Use this for document photograph requirements.

- Width: `160 px`
- Height options: `200 px`, `206 px`, or `212 px`
- Target size: `5 KB to 20 KB`
- Output: `resized-photo.jpg`

### Signature

Use this for signature upload requirements.

- Width: `256 px`
- Height: `64 px`
- Target size: `5 KB to 20 KB`
- Output: `resized-signature.jpg`

### Image 75-100 KB

Use this when a form or portal asks for an image between `75 KB` and `100 KB`.

- Keeps the source image aspect ratio.
- Uses crop selection if enabled.
- Compresses the final JPEG to fit under `100 KB` when possible.
- Warns if the image is too simple or too small to naturally reach `75 KB`.
- Output: `resized-image-75-100kb.jpg`

### PDF

Use this when a portal asks for a PDF between `75 KB` and `100 KB`.

- Accepts `.pdf` files.
- Optimizes PDF structure in the browser.
- Confirms when the output is within `75 KB to 100 KB`.
- Warns when the PDF cannot be brought into that exact range.
- Output: `compressed-file.pdf`

PDF compression uses `pdf-lib` from a CDN, so internet access is required for PDF mode.

## Crop Workflow

1. Select an image mode: `Photograph`, `Signature`, or `Image 75-100 KB`.
2. Upload a JPEG or PNG file.
3. Enable `Crop image before resize`.
4. Drag on the source image to select the part you want to keep.
5. Click `Resize image`.
6. Download the generated file.

The crop is applied before resizing and compression.

## Supported Files

Supported:

- `.jpg`
- `.jpeg`
- `.png`
- `.pdf`

Not supported:

- `.tif`
- `.tiff`

Convert TIFF files to JPEG or PNG before uploading.

## Project Files

- `index.html`: Main browser application.
- `server.js`: Local Node.js web server.
- `package.json`: Start script for the web app.
- `README.md`: User instructions.
- `APPROACH_AND_LINKEDIN.md`: Build approach and shareable LinkedIn post.

## Notes

- Image processing is handled with browser canvas APIs.
- PDF optimization is best-effort. Some PDFs need deeper image recompression to hit an exact file-size range.
- No uploaded file is sent to a backend server by this app.
