# Automatic-Metadata-generator
### A model to automatically generate metadata for unstructured documents. 
### Build during MaRS Open Projects 2025

# Project Overview
The project aims to develop a system which could generate metadata of any unstructured document like pdf, docs etc automatically. This system will enhance document discoverability, classification, and subsequent analysis by producing scalable, consistent, and semantically rich metadata.
The model supports different inputs like .pdf, .doc, .docx, various images (.jpg, .png, .tiff, .gif, .jpeg).
Perform an efficient image preprocessing using OpenCV (rescaling, denoising, binarization, border handling).
Uses Tesseract OCR engine to extract accurate text even for the documents which could not be digitally selected and used.
Extracts high-level document metadata using natural language techniques.
Options to save and visulize the intermediate results as well.

# Output Features
1. Prints the metadata of the uploaded document.
2. The metadata includes
   **File Name**
   **Title**
   **Date and time of Extraction**
   **Type of document**
   **No. of words**
   **No. of characters**
   **Keywords**
   **Summary**
   **Emails and phone no.**
   
4. Option to extract the complete text from the given file.

# Steps to reach the results
The model is divided majorly into three parts .
  **UniversalOCRProcessor**
  **OCRImageProcessor**
  **OCRMetadataExtracto**
The UniversalOCRProcessor makes the given document ready for processing and metadata generation. 
When any document is submitted, the UniversalOCRProcessor first checks for its extensions and accordingly converts it into image format. e.g., If the document is .pdf, it utilizes convert_from_path imported from pdf2image, similiarly a different function for the documents which makes use of docs, docs.shared and aspose.words.
After conversion of the document pages to image it is passed to OCRImageProcessor to process each image for accurate meta data generation.
OCRImageProcessor utilizes OpenCV. 
First it converts the BGR image into the standard OpenCV format.
The techniques used in image processing are -->
  **Rescaling**      : The default scale factor is 2.
  **Binarization**   : The method used is 'otsu' with threshold value of 127.
  **Denoising**      : 'median' method with a kernel size of 5.
  **Handling Borders**
The function "preprocess_complete" completely processes the image in this class.
The next Class "OCRMetadataExtractor" is for metadata extraction.
Certain words like 'at', 'the' , 'a', 'an' etc. are listed as stopwords which could be used for keyword and summary genertion.
There are multiple functions under this class which performs different extractions.
1. Title Extraction: This function uses certain strategies to extract the title. It searches for capital sentances, all capital words, numbered titles etc. to identify the title. pattern is matched by 're'.
2. Date Extraction: date pattern is matched. any string matching the pattern is identified
3. Keyword Extraction: The string of length more or equal to 4 and excluding the words which do not carry significant meaning like 'a', 'an', 'the' etc. are selected.
4. Generating Summary: It breaks sentences on ., !, ? etc and filters out the sentences having less than 30 words and then joins sentences.
5. Emails, Phone no. are also matched according to identified patterns.
Then the document is classified according to the extracted keywords.
Next functions are majorly for viewing the OCR and the metadata.
The model is flexible, in the codes, some lines can be uncommented to print the extracted text as well.
