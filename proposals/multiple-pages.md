# Multiple Pages

*Status*: proposal

*Author*: Tina Russell

*Submission date*: 5 August, 2010

*Implementations*: none

One OpenRaster file would be able to contain more than one page. This would be
useful for things like sketchbooks, comic stories, or anything else where
you might want to have raster images in sequence but where using separate image
files would be impractical.

Any program which does not support multi-page OpenRaster files would simply
show the first page and hide the others (or, perhaps, show a page defined as
“default” by the image author).

Any program could implement, crudely, something like this using layer groups.
However, that would be unlikely to be recognized across implementations.