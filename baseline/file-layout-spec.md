# File Layout Specification

## Storage

OpenRaster files have the file name extension .ora. The data is stored within
a [Zip file](http://www.pkware.com/documents/casestudies/APPNOTE.TXT) wrapper.

Technical note: Zip files can use a different compression algorithm on a
per-file basis. Only DEFLATED and STORED should be used for OpenRaster files.
The file names within OpenRaster "zip files" are case-sensitive and must be
UTF-8 encoded. If you decide to write non-ascii file names, be aware that the
Zip format has an UTF-8 flag for each file name. Make sure this flag is set,
otherwise the content might get decoded as cp437. Note also that the Zip file
format has a 64 bit extension which must be enabled to write files larger than
4GB.

This file format is hereafter referred to as an "OpenRaster file" in this
specification. It has a number of required and optional standard files and
subdirectories within it:

	example.ora  [considered as a folder-like object]
	 ├ mimetype
	 ├ stack.xml
	 ├ data/
	 │  ├ [image data files referenced by stack.xml, typically layer*.png]
	 │  └ [other data files, indexed elsewhere]
	 ├ Thumbnails/
	 │  └ thumbnail.png
	 └ mergedimage.png

## Required Files

### mimetype

The first file in the archive must be called `"mimetype"`, without a file name
extension. It must be STORED without compression. This file must contain the
string `"image/openraster"`, with no whitespace or trailing newline.

### stack.xml

This is the UTF-8 encoded XML file from the Layers Stack Specification.

### data

This directory is (by convention) where data files are usually stored. Files
must be referenced in stack.xml by their full path relative to the OpenRaster
file's root, e.g. `"data/layer2.png"`.

All files inside this directory should be referenced from somewhere. Well known
file name extensions (like `.png`) should be lower case. 

### Thumbnails/thumbnail.png

An OpenRaster file must have a `thumbnail.png` in order to allow file browser
software to render the thumbnail efficiently. It must be a non-interlaced PNG
with 8 bits per channel of at most 256x256 pixels. It should be as big as
possible without upscaling or changing the aspect ratio. Any aspect ratio is
permitted. It should not contain any frame or decoration.

The thumbnail file must not be referenced in any `.xml` file. 

## Optional Files

### mergedimage.png

An OpenRaster file must have a `mergedimage.png` in order to accommodate viewer
software. It must contain the final rendered image without any frame or
decoration. This file must be a PNG file with 8 or 16 bits per channel. *(Since
0.0.2, this file is mandatory.)*