# Build Approach and LinkedIn Post

This document explains how the app was built in simple terms, followed by a LinkedIn post you can share.

## Step-by-Step Build Approach

### 1. Started With the Real Upload Requirements

The first requirement was not just "resize an image." It had specific portal-style constraints:

- Photograph format: JPEG/PNG.
- Photograph dimensions: `160 px` wide and `200-212 px` high.
- Signature dimensions: `256 x 64 px`.
- Photograph/signature size: `5 KB to 20 KB`.
- Later requirement: image/PDF upload size between `75 KB and 100 KB`.

So the app was designed around real upload rules instead of a generic compressor.

### 2. Built a Browser-First Tool

The first version was a standalone `index.html` file. This made it simple to use because no framework or installation was needed.

The image work happens in the browser using:

- `FileReader` to read uploaded files.
- `Image` to load JPEG/PNG files.
- `canvas` to crop, resize, sharpen, and export images.
- `toDataURL('image/jpeg', quality)` to control JPEG compression.

### 3. Added Exact Image Modes

The app now has separate modes so each upload rule stays clear:

- `Photograph`: exact `160 x 200`, `160 x 206`, or `160 x 212 px`.
- `Signature`: exact `256 x 64 px`.
- `Image 75-100 KB`: keeps aspect ratio and targets the larger upload-size range.
- `PDF`: checks and optimizes PDF files for `75 KB to 100 KB`.

This avoids mixing rules. For example, signature resizing should not accidentally use the 75-100 KB image rule.

### 4. Preserved Aspect Ratio Without Breaking Required Dimensions

For photo and signature uploads, the final canvas must still be the exact required size.

So when `Maintain aspect ratio` is enabled:

- The image is fitted inside the required output size.
- The remaining space is filled with a white background.
- The final file still has the exact required width and height.

This prevents distortion while still satisfying strict upload dimensions.

### 5. Improved Quality for Small Text and Signatures

Directly shrinking an image can make text and signatures look soft.

To reduce blur, the app uses:

- Progressive downscaling instead of one large resize step.
- High-quality canvas image smoothing.
- A light sharpening pass after resizing.
- Stronger sharpening for signature mode.

This keeps small text and signature strokes clearer after compression.

### 6. Added Crop Before Resize

Cropping was added because many upload failures happen when the original image has too much background.

The crop flow is:

1. Upload an image.
2. Enable crop.
3. Drag over the source image.
4. Resize from the selected crop area.

The app crops first, then resizes and compresses. That gives better control over the final result.

### 7. Added PDF Handling

PDF support was added as a separate mode because PDFs are different from images.

The app uses `pdf-lib` in the browser to:

- Load the PDF.
- Re-save it with object streams.
- Compare original and optimized sizes.
- Report whether the result is within `75 KB to 100 KB`.

Important limitation: browser-side PDF optimization cannot always recompress embedded images deeply enough to hit an exact size. The app reports this clearly instead of pretending every PDF can be forced into the range.

### 8. Turned It Into a Local Web App

The app started as a direct HTML file and was then wrapped with a tiny Node.js server.

Files added:

- `server.js`: serves the app locally.
- `package.json`: adds the `npm start` command.

Now it can be opened as:

```text
http://127.0.0.1:3000/
```

This makes it feel like a normal web application while still keeping everything local.

### 9. Kept the Tool Lightweight

The app intentionally avoids a heavy frontend framework.

That keeps it:

- Easy to run.
- Easy to share.
- Easy to inspect.
- Fast enough for small document-upload tasks.

## Technical Summary

- Frontend: HTML, CSS, JavaScript.
- Image processing: browser canvas.
- PDF optimization: `pdf-lib` CDN.
- Local server: Node.js built-in `http` module.
- Runtime command: `npm start`.

## LinkedIn Post

I built a small local web app to solve a problem almost everyone faces at some point:

Getting photos, signatures, images, and PDFs to match strict upload portal requirements.

You know the kind:

- Photo must be `160 x 200-212 px`
- Signature must be `256 x 64 px`
- File size must be `5 KB to 20 KB`
- Another upload must be between `75 KB and 100 KB`
- PDF must also fit a size range

Instead of using random online tools and uploading personal documents to unknown websites, I built a browser-based local tool that handles it on my machine.

What it does:

- Resizes document photos to exact dimensions
- Resizes signatures to exact dimensions
- Maintains aspect ratio
- Lets you crop before resizing
- Compresses images into required size ranges
- Checks and optimizes PDFs for `75 KB to 100 KB`
- Runs locally as a simple web app

The interesting part was balancing two things that often conflict:

1. The final image must have exact dimensions.
2. The image should not look stretched, blurry, or distorted.

So I added aspect-ratio fitting, progressive downscaling, light sharpening, crop-before-resize, and clear file-size feedback.

This is a good reminder that useful software does not always need to be big. Sometimes the best tool is a small one that removes a repeated frustration.

If anyone wants the markdown notes or the approach I followed, happy to share.

#BuildInPublic #WebDevelopment #JavaScript #Productivity #LocalTools #DocumentUpload #FrontendDevelopment
