Scan documents and images using scanimage(1) from the SANE distribution and
tesseract(1) for OCR and PDF export.

The scanner package uses two sets of customizations for image mode and
document mode, with the former usually configured to use high resolution
and an image file format, like JPEG, and the latter to use lower resolution
and a document format, like PDF or text.  The available file formats are
provided by scanimage(1) for image mode and tesseract(1) for document mode.
The scanner package uses tesseract(1) to provide optical character
recognition (OCR).  You can select the language plugins with
‘scanner-tesseract-languages’.

In document mode, you can scan one or multiple pages that are then written
in a customizable output format, e.g. (searchable) PDF or text, or whatever
tesseract provides.  You can also customize resolution, intermediate image
format, and paper size.  The command ‘scanner-scan-document’ starts a
document scan.  Without a prefix argument, it scans one page.  With a
non-numeric argument, it asks the user after each scanned page for
confirmation to scan another page.  With a numeric argument, it scans that
many pages.  In the latter case, it observes a delay between scans that is
customizable using ‘scanner-scan-delay’.

The ‘scanner-scan-image’ command performs one scan or multiple scans in
image mode.  This function tries to guess the file format from the chosen
file name or falls back to the configured default, see
‘scanner-image-format’.  The prefix argument works as in document mode.

The scanning commands are also available in the Tools->Scanner menu.

For both images and documents, you can customize the scan mode
(e.g. "Color" or "Gray") if your scanning device supports it.

Finally, you can pass additional options to the backends using the
customization variables ‘scanner-scanimage-switches’ and
‘scanner-tesseract-switches’.  The former variable is helpful for tuning
brightness and contrast, for instance.