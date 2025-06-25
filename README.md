
# Automatic Metadata Generator

###  A model to automatically generate metadata for unstructured documents  
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

##  Output Features

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
# Requirements
Django==5.0.0
opencv-python==4.9.0.80
numpy>=1.26.0
matplotlib>=3.8.0
scipy>=1.11.4
scikit-image>=0.22.0
pytesseract==0.3.10
pdf2image==1.16.3
PyMuPDF==1.23.5
python-docx==0.8.11
Pillow>=10.0.0


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

# Major Function Description
1. Title Extraction: This function uses certain strategies to extract the title. It searches for capital sentances, all capital words, numbered titles etc. to identify the title. pattern is matched by 're'.
2. Date Extraction: date pattern is matched. any string matching the pattern is identified
3. Keyword Extraction: The string of length more or equal to 4 and excluding the words which do not carry significant meaning like 'a', 'an', 'the' etc. are selected.
4. Generating Summary: It breaks sentences on ., !, ? etc and filters out the sentences having less than 30 words and then joins sentences.
5. Emails, Phone no. are also matched according to identified patterns.
   
Then the document is classified according to the extracted keywords.
Next functions are majorly for viewing the OCR and the metadata.
The model is flexible, in the codes, some lines can be uncommented to print the extracted text as well.


