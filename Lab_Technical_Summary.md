# Comprehensive Technical Summary — Labs 1–5 (Image Processing)

---

## Lab 1: Image Loading, Display & Basic Properties

### Libraries & Dependencies

| Library | Role |
|---------|------|
| **OpenCV (`cv2`)** | Reads/writes images, provides BGR format arrays |
| **Pillow (`PIL.Image`)** | Alternative image I/O with RGB format |
| **Matplotlib** | Inline display of images |
| **NumPy** | Array inspection (shape, dtype) |

### Classes

No custom classes are defined. The lab relies on OpenCV's `ndarray` representation and Pillow's `Image` object.

### Key Functions / Methods

- **`cv2.imread()` / `Image.open()`** — Load an image from disk into memory (NumPy array or PIL Image, respectively).
- **`cv2.imwrite()` / `Image.save()`** — Write an image back to a file in a specified format.
- **`plt.imshow()` / `plt.axis('off')`** — Render an image inline; hide axes for cleaner display.
- **`np.array()`** — Convert a PIL Image to a NumPy array so that `.shape` and `.dtype` can be inspected.

### Overall Benefit

Introduces the two most common Python image I/O ecosystems (OpenCV and Pillow), demonstrates how to load, display, save, and inspect basic image metadata (dimensions, channels, data type). It establishes the foundational workflow used by every subsequent lab.

---

## Lab 2: Sampling, Quantization & Pixel-Level Arithmetic

### Libraries & Dependencies

| Library | Role |
|---------|------|
| **OpenCV (`cv2`)** | Image reading, resizing (downsampling), absolute difference, bitwise operations |
| **NumPy** | Array arithmetic, clipping, type casting |
| **Pillow (`PIL.Image`)** | Secondary I/O and resizing with Lanczos filter |
| **Matplotlib** | Side-by-side plotting |

### Classes

No custom classes.

### Key Functions / Methods

- **`sample_image(image, factor)`** — Downsamples an image by a given factor using nearest-neighbor interpolation (`cv2.resize` with `INTER_NEAREST`), simulating reduced spatial resolution.
- **`quantize_image(image, levels)`** — Reduces the number of grayscale levels by integer division and rescaling, demonstrating the effect of bit-depth reduction.
- **`plot_images(original, sampled, quantized)`** — Utility that displays original, sampled, and quantized images in a 1×3 subplot grid.
- **Pixel arithmetic block** — Performs addition, subtraction (with `np.clip`), scalar addition, absolute difference (`cv2.absdiff`), bitwise AND, and bitwise OR on image pairs to explore set/logical operations at the pixel level.

### Overall Benefit

Demonstrates how spatial resolution (sampling) and intensity resolution (quantization) affect image quality. It also covers pixel-wise arithmetic and logical operations, which are building blocks for image blending, masking, and change detection.

---

## Lab 3: Color Spaces, Geometric Transforms & Intensity Transforms

### Libraries & Dependencies

| Library | Role |
|---------|------|
| **OpenCV (`cv2`)** | Color conversion, resizing, perspective warp |
| **imutils** | Convenient image rotation |
| **NumPy** | Transformation matrices, power/log math |
| **Matplotlib** | Display of transform results |

### Classes

No custom classes.

### Key Functions / Methods

- **`cv2.cvtColor()`** — Converts between color spaces (BGR → Grayscale, BGR → YUV, BGR → RGB), with individual Y/U/V channel visualization.
- **`cv2.resize()` with `INTER_AREA`** — Shrinks an image to a target size using area-based interpolation.
- **`imutils.rotate()`** — Rotates an image by an arbitrary angle with automatic bounding-box adjustment.
- **`cv2.warpPerspective()`** — Applies a 3×3 homography matrix to perform shearing/perspective distortion.
- **Negative transform** — Computes `255 − image` to invert intensities.
- **Log transform** — Applies `c · log(1 + image)` to expand dark tones and compress bright tones.
- **Gamma (power-law) correction** — Applies `(image / 255)^γ × 255` with γ = 2 and γ = 4 to control brightness non-linearly.

### Overall Benefit

Covers the two major families of spatial-domain operations: geometric transforms (resize, rotate, shear) and intensity transforms (negative, log, gamma). Understanding these is essential for pre-processing pipelines in computer vision and medical imaging.

---

## Lab 4: Thresholding, Contrast Stretching & Histogram Processing

### Libraries & Dependencies

| Library | Role |
|---------|------|
| **OpenCV (`cv2`)** | Binary thresholding |
| **scikit-image (`skimage.exposure`, `skimage.data`)** | Contrast stretching, histogram equalization, histogram matching; built-in sample images |
| **NumPy** | Percentile computation |
| **Matplotlib** | Image + histogram subplot visualization |

### Classes

No custom classes.

### Key Functions / Methods

- **`cv2.threshold()`** — Applies binary thresholding at multiple threshold values (0, 50, 100, 150, 200) to segment an image.
- **`np.percentile()`** — Computes the 2nd/98th (and 3rd/80th) percentiles to define the input range for contrast stretching.
- **`exposure.rescale_intensity()`** — Linearly stretches pixel values between the computed percentiles to fill the full dynamic range.
- **`exposure.equalize_hist()`** — Performs global histogram equalization, flattening the histogram to improve contrast uniformly.
- **`exposure.match_histograms()`** — Adjusts a source image's histogram to match a reference image's histogram, useful for color/tone transfer.

### Overall Benefit

Teaches histogram-based enhancement techniques that are fundamental to improving image contrast and performing simple segmentation. The progression from manual thresholding → contrast stretching → equalization → histogram matching covers the standard toolkit for intensity-based image improvement.

---

## Lab 5: Spatial Filtering — Smoothing & Sharpening

### Libraries & Dependencies

| Library | Role |
|---------|------|
| **OpenCV (`cv2`)** | Gaussian blur, custom convolution (`filter2D`), Laplacian edge detection |
| **NumPy** | Kernel construction, float conversion, clipping |
| **Matplotlib** | Comparative image display |

### Classes

No custom classes.

### Key Functions / Methods

- **Unsharp Masking block** — Blurs the image with `cv2.GaussianBlur`, computes the difference (detail mask), then adds it back scaled by an `amount` parameter to sharpen the image.
- **`cv2.filter2D()`** — Convolves the image with a custom 7×7 averaging kernel for box-blur smoothing.
- **`cv2.GaussianBlur()` (5×5 vs 21×21)** — Compares the effect of different kernel sizes on smoothing intensity.
- **`cv2.Laplacian()`** — Computes the second-derivative (Laplacian) of the image to detect edges, then subtracts the result from the original to sharpen.

### Overall Benefit

Demonstrates the core spatial filtering operations: smoothing (averaging, Gaussian) for noise reduction and sharpening (unsharp masking, Laplacian) for edge enhancement. These convolution-based techniques are the backbone of image pre-processing in virtually every computer vision pipeline.

---

## Cross-Lab Summary

| Lab | Theme | Core Concept |
|-----|-------|--------------|
| 1 | I/O & Inspection | Loading, saving, and examining image properties |
| 2 | Resolution & Arithmetic | Sampling, quantization, pixel-level math |
| 3 | Transforms | Color spaces, geometric & intensity transforms |
| 4 | Histograms | Thresholding, contrast stretching, equalization, matching |
| 5 | Filtering | Smoothing and sharpening via convolution |

Together, the five labs form a progressive introduction to **digital image processing**: starting from basic I/O, moving through resolution and intensity manipulation, then geometric/intensity transforms, histogram-based enhancement, and finally spatial-domain filtering. The shared dependency stack — OpenCV, NumPy, Matplotlib, scikit-image, and Pillow — is the standard Python toolkit for this domain.
