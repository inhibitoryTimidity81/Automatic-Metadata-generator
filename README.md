
# Automatic Metadata Generator

### 🔧 A model to automatically generate metadata for unstructured documents  
**Built during MaRS Open Projects 2025**

---

## Project Overview

This project aims to build a system that **automatically generates metadata from unstructured documents** such as PDFs, DOCX, images, and TXT files. The primary goal is to enhance document **discoverability**, **classification**, and **semantic analysis** by producing scalable, consistent, and meaningful metadata.

### Core Capabilities:
- Supports input formats: `.pdf`, `.doc`, `.docx`, `.txt`, `.jpg`, `.jpeg`, `.png`, `.tiff`, `.gif`
- Uses **Tesseract OCR** to extract text, even from scanned or image-based documents
- Applies **OpenCV-based image preprocessing** for enhanced OCR accuracy
- Performs **semantic metadata extraction** using regex and NLP techniques
- Allows **saving and visualization** of intermediate and final results

---

## 🎯 Output Features

When a file is processed, the system provides:

-  **Complete metadata**, including:
  - **File name**
  - **Title**
  - **Date and time of extraction**
  - **Document type**
  - **Word count**
  - **Character count**
  - **Keywords**
  - **Summary**
  - **Emails and phone numbers (if any)**

-  **Full text extraction** from the document (optional)
-  **Page-wise intermediate images and text files** (if `save_results=True`)

---

## How It Works

The project is structured around **three main modules**:

### 1. `UniversalOCRProcessor`
- Detects file type based on extension
- Converts documents to image format:
  - PDFs: `pdf2image.convert_from_path`
  - DOCX: `aspose.words`
  - TXT: Synthetic image rendering using `PIL`
- Outputs a list of images for downstream OCR

---

### 2. `OCRImageProcessor`
- Processes images using OpenCV:
  - **Rescaling**: Enlarges image (default scale = `2.0`)
  - **Binarization**: Applies Otsu thresholding
  - **Denoising**: Median blur with kernel size `5`
  - **Border handling**: Removes and adds borders to avoid edge cut-off
- All preprocessing steps are encapsulated in the `preprocess_complete()` method

---

### 3. `OCRMetadataExtractor`
- Extracts semantic metadata from the OCR text:
  - **Title**: Identifies potential title using regex on headers, numbered sections, etc.
  - **Date**: Extracts date strings using multiple date patterns
  - **Keywords**: Filters meaningful words, removes stopwords
  - **Summary**: Compiles meaningful sentences to summarize the content
  - **Contact Info**: Finds email addresses and phone numbers
  - **Document Type**: Classifies as Invoice, Certificate, Report, etc.

---

## Usage

### 1. Extract Text Only

```python
from main import universal_ocr

text = universal_ocr("example.pdf", show_images=False, save_results=True)
print(text)

2. Extract Text + Metadata
python
Copy
Edit
from main import universal_ocr_with_metadata, print_metadata

text, metadata = universal_ocr_with_metadata("example.docx")
print_metadata(metadata)

 Sample Output
ocr_results/
├── page_001_processed.png
├── page_001_text.txt
├── ...
├── complete_extracted_text.txt
