# Animation

*Status*: proposal

*Author*: Efa (& others?)

*Submission date*: 19 October, 2010

*Implementations*: none

One OpenRaster file would be able to contain more than one image. This would
be useful for things like sketchbooks, comic stories, or anything else where
you might want to have raster images in sequence but where using separate image
files would be impractical.

File contains timing for every raster image, and a jump/loop count settings.

Any program that does not support animation OpenRaster files would simply show
the first raster and hide the others (or, perhaps, show a raster defined as
“default” by the image author).

Any program could implement, crudely, something like this using layer groups.
However, that would be unlikely to be recognized across implementations.