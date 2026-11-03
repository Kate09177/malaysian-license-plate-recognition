# Malaysian License Plate Recognition System
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Kate09177/malaysian-license-plate-recognition/blob/main/IPPR_Assignment.ipynb)
An end-to-end computer vision pipeline that detects vehicle license plates in an image, reads the plate text via OCR, corrects common character-recognition errors, and identifies the plate's issuing state/region — covering Malaysian plates (all states, plus military, diplomatic, police, VIP, and tricycle plate types) and Singapore plates.
Built as a group coursework assignment (CT036-3-IPPR).

## How it works
**1. Plate detection (classical CV, no deep learning)**
- Top-Hat morphology highlights small bright text against the plate's background, using separate kernels tuned for single-line and two-line plate layouts.
- Sobel edge detection locates the dense horizontal edges typical of plate text.
- Morphological closing + Otsu thresholding + contour filtering (by aspect ratio and area) isolates plate-shaped regions.
- Nearby boxes are merged to handle two-row plates correctly.

**2. Text extraction (OCR)**
- The cropped plate region is preprocessed multiple ways in parallel — CLAHE contrast enhancement, sharpening, Otsu binarization, adaptive thresholding — and each version is run through [EasyOCR](https://github.com/JaidedAI/EasyOCR).
- The highest-confidence OCR result across all preprocessing variants is kept.

**3. OCR error correction**
- Common look-alike character confusions (e.g. `W`/`N`/`K`/`H`/`M`) are corrected using a constrained search against valid Malaysian plate prefixes.
- Digit/letter substitution rules (`0↔O`, `1↔I`, `5↔S`, `8↔B`, etc.) clean up the prefix and number segments separately.
- A confidence-scoring function filters out low-quality or implausible detections before they're shown.

**4. State / region identification**
- Plate prefixes are matched against a full lookup table covering every Malaysian state, plus special categories (Military, Diplomatic Corps, Police, VIP, Tricycle).
- A separate rule set detects Singapore-format plates.

**5. Interactive demo**
- A [Gradio](https://gradio.app/) web UI lets you upload any image and see the annotated result (bounding boxes + plate text + identified state) alongside a readable results summary.

## Tech stack
`Python` · `OpenCV` · `EasyOCR` · `imutils` · `NumPy` · `Gradio`

## Running it
The notebook is self-contained — click the **Open in Colab** badge above, run all cells top to bottom, and the Gradio demo will launch with a shareable link at the end.

## Notes
- Detection is purely classical/rule-based computer vision (no trained CNN for plate localization) — tuned specifically for Malaysian plate proportions and layouts.
- Test images used during development were sourced from publicly available internet images, used here for educational/non-commercial purposes only.
- Detection is purely classical/rule-based computer vision (no trained CNN for plate localization) — tuned specifically for Malaysian plate proportions and layouts.
- Test images used during development were sourced from publicly available internet images, used here for educational/non-commercial purposes only.
