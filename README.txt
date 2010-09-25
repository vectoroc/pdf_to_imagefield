* PDF To ImageField *

The PDF To ImageField module provides functionality of automatic conversion of 
uploaded PDF files to images. It creates image for every page of source PDF file.
Module is implemented as widget for FileField and uses one more ImageField field
for storing result of conversion - target ImageField must be another field of the
same content type, and be configured to allow unlimited values.

* Settings

PDF widget has two settings:

1) ImageField to use as target of conversion
2) Density (horizontal and vertical) to use when rendering PDF

* Processing *

Actual processing is perfomed using ImageAPI's ImageMagick toolkit.
GD is not supported.

Processing of PDF may take considerable time. On this reason, it is not performed 
at node save stage, insted its launched via cron. Amount of PDF files to process 
per cron run is configured on the module's settings page. 

After processing is done, there is no any relation between source PDF and target ImageFields.

* Use of ImageMagick *

The only candidate function from ImageAPI which may perform PDF to image 
convertion is _imageapi_imagemagick_convert(). Unfortunately, it can't pass 
arguments to 'convert' tool of ImageMagick *before* source file specification, 
which is needed to change default density of 100x100dpi. 
On this reason, a custom functionis included in the module's source:

function pdf_to_imagefield_convert_pdf($source, $dest, $args = array(), $extra = array()) {
  . . .
}

