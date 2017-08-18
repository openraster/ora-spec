# PNG Data Requirements

*Status*: proposal

*Author*: unknown

*Submission date*: unknown

*Implementations*: unknown

The OpenRaster specification should place certain limitations on image data
within `.ora` containers in order to ensure that the widest range of
applications can open them and display them correctly.

## General PNG data requirements

*Current specification*: File Layout

*Change*: new section

Unless otherwise specified for a particular kind of PNG data stream in an
OpenRaster file,

* PNG data should not be interlaced (OpenRaster is not a streaming format)
* Applications reading OpenRaster data should read and process colour management
chunks present in PNG files as described in the
[PNG specification](http://www.w3.org/TR/PNG/#4Concepts.ColourSpaces).
  * This specification does not place requirements on how applications should use
  this colour management information internally.
  * It may however express filters or layer blending modes in terms of particular
  colour spaces.
* Applications writing OpenRaster files *should* write appropriate colour management
chunks to each PNG file in the wrapper.

## Layer data requirements

*Current specification*: File Layout ('Data' section)

*Change*: addendum

Stored data files for layers should be encoded in PNG format.

## Thumbnails/thumbnail.png

*Current specification*: ../FileLayout#Thumbnails.2BAC8-thumbnail.png

*Change*: addendum

Thumbnail images *should* be encoded in the sRGB colour space. They should be
written with an sRGB chunk (and the "Perceptual" rendering intent), and with
the canonical `cHRM` and `gAMA` chunks and data values for the sRGB encoding
colour space.