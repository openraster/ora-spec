# Layer Stack Specification

Note: this is an early draft of the specification, do not take anything on this
page for granted. Pretty much everything is open for discussion. 

## Introduction

This document specifies the [OpenRaster](https://github.com/openraster/ora-spec)
Layer Stack XML format. The main two goals of the specification are
interoperability between applications and allowing files to be saved without
destructive pixel operations.

Note that this document does not define how the data is stored, and especially
doesn't address the issue of how to store pixels nor how OpenRaster files can
be stored on disk. This document doesn't address the mathematics behind layer
compositing operations: it's simpler to describe that using other standard
bodies' documents at this stage.

The first part of the document explains the concept of a baseline Layer Stack,
with examples. Each attribute and element defined by the baseline profile is
explained individually.

## Baseline documents

A baseline OpenRaster document is a document that can be opened and look
similar on all applications and platforms. The baseline layer stack format
defines the minimum requirement to achieve interoperability and what is unsafe
for interoperability. Layer Stack format permits extensions, which should be
individually specified in detail, and work safely with the baseline profile.

Some users will require full interoperability, to create files which can be
edited on every application supporting OpenRaster. Other users will only need
to view OpenRaster files. This specification defines two classes of baseline
support:

* "Full baseline", which allows reading and editing
* "Viewing baseline" which guarantees visualization of the file, but editing
might damage the result

## Syntax

The formal syntax of the layer stack is given following the

* https://gitorious.org/openraster/openraster-standard/blobs/master/schema.rnc (compact format, canonical schema)
* https://gitorious.org/openraster/openraster-standard/blobs/master/schema.rng (xml format)

## Example

	<?xml version='1.0' encoding='UTF-8'?>
	<image version="0.0.3" w="300" h="177" xres="600" yres="600">
	  <stack>
	    <stack x="10">
	      <stack>
	        <layer name="OpenRaster Logo" src="data/hw.svg" x="5" y="5" />
	        <text x="50" y="10">Use a Rich Text XML Specification to write cool text in your OpenRaster File</text>
	      </stack>
	    </stack>
	    <!-- filters are syntactically permissible, but not valid for baseline -->
	    <layer name="layer1" src="data/image1.png" />
	  </stack>
	</image>

## Elements and Attributes

### image element

This is the root element of the file. The logical size of the image is given
by the mandatory `"w"` and `"h"` attributes, which are positive integers. The
content expressed within `image` may have extents which can be smaller or
larger, and so the image should be cropped to (0,0,w,h) when displaying,
printing, or otherwise exporting to a context which requires a rectangular
image.

* The mandatory `"version"` attribute specifies the version of the OpenRaster
specification to which the OpenRaster file as a whole conforms. Values are
[xsd:string](http://www.w3.org/TR/xmlschema-2/#string)s which conform to
[SemVer 2.0.0](http://semver.org/spec/v2.0.0.html). (Since: 0.0.1)
* The optional `xres` and `yres` attributes specify the nominal resolution of
the document as a whole in the horizontal and vertical dimensions, in pixels
per inch. Values are [xsd:int](http://www.w3.org/TR/xmlschema-2/#int)s with a
minimum value of 1. The default value, if unspecified, is 72. If either is
specified, the other must also be specified. Applications should preserve
resolution information specified in the document in both directions unless it
is adjusted by the user, even if they make the simplifying assumption that
`xres` is equal to `yres`. *(Since: 0.0.3)*

### Common attributes for layer and non-root stack elements

#### x and y attributes

Position attributes. These are signed integers with a default value of 0.

#### name attribute

The name of the object represented by an element. A Unicode string, encoded in
the XML document's own encoding.

#### opacity attribute

The object's opacity, expressed as a simple floating-point number. Values
between 0.0 for fully transparent, and 1.0 for fully opaque.

#### visibility attribute

The visibility of the object.

* Valid values are either visible, or hidden.
* Default value: visible.

#### composite-op attribute

The operation to use when rendering this stack or layer over its backdrop.
The effect is to be calculated as described in 
[Compositing-1: General Formula for Compositing and Blending](http://www.w3.org/TR/compositing-1/#generalformula),
namely as combinations of a colour *blending function* and a Porter-Duff alpha
compositing operator. This attribute's name is perhaps a misnomer now, but it
reflects the history of the OpenRaster specification and the evolution of the
attribute from its inspirations in the earlier
[SVG 1.2 Working Draft](http://dev.w3.org/SVG/modules/compositing/master/SVGCompositing.html#comp-op-property)
and in [GEGL's named processing "operations"](http://www.gegl.org/operations.html),
which also provide the seeds of the naming scheme for values below.

<table border="1" cellpadding="5" cellspacing="0">
<tr>
  <th>Value</th>
  <th>Blending function</th>
  <th>Compositing Operator</th>
</tr>
<tr>
  <td>svg:src-over</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingnormal">Normal</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:multiply</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingmultiply">Multiply</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:screen</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingscreen">Screen</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:overlay</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingoverlay">Overlay</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:darken</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingdarken">Darken</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:lighten</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendinglighten">Lighten</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:color-dodge</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingcolordodge">Color Dodge</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:color-burn</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingcolorburn">Color Burn</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:hard-light</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendinghardlight">Hard Light</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:soft-light</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingsoftlight">Soft Light</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:difference</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingdifference">Difference</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:color</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingcolor">Color</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:luminosity</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingluminosity">Luminosity</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:hue</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendinghue">Hue</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:saturation</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingsaturation">Saturation</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover">Source Over</a></td>
</tr>
<tr>
  <td>svg:plus</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingnormal">Normal</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_plus">Lighter</a></td>
</tr>
<tr>
  <td>svg:dst-in</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingnormal">Normal</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_dstin">Destination In</a></td>
</tr>
<tr>
  <td>svg:dst-out</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingnormal">Normal</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_dstout">Destination Out</a></td>
</tr>

<tr>
  <td>svg:src-atop</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingnormal">Normal</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcatop">Source Atop</a></td>
</tr>
<tr>
  <td>svg:dst-atop</td>
  <td><a href="http://www.w3.org/TR/compositing-1/#blendingnormal">Normal</a></td>
  <td><a href="http://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_dstatop">Destination Atop</a></td>
</tr>
</table>

The default value is `svg:src-over`, which represents simple alpha compositing.

In the future other compositing modes might be added, and a way for applications
to define new modes will be specified.

#### stack element

The `stack` element describes a group of layers. They may contain sub-`stack`s,
`layer`s, or `text` elements. The first element in a stack is the uppermost.

The following attributes are optional on non-root `stack`s, but must be omitted
on the root stack.

* `name`
* `x and y`
* `opacity`
* `visibility`
* `composite-op`


#### layer element

The `layer` element defines a graphical layer within a layer stack, stored in a
separate file within the OpenRaster file. The following attribute is required:

* `"src"`: the path to the stored data file for this layer. See the File Layout
Specification for an explanation of the values which can go here.

The following attributes are optional on `layer` elements:

* `name`
* `x and y`
* `opacity`
* `visibility`
* `composite-op`

#### text element

TODO: define it! Ideally, use another rich text specification, e.g. a relevant
subset of the OpenDocument Text specification or XHTML.

### Compositing the image

Layer stacks should be composited in a manner conforming to the W3C's
[Compositing and Blending Level 1 Candidate Recommendation](http://www.w3.org/TR/compositing-1/).
In terms of this specification's rendering model, some OpenRaster layer stacks
or nested sub-stacks are *isolated* groups, but some sub-stacks may be
non-isolated.

[Isolated groups](http://www.w3.org/TR/compositing-1/#isolatedgroups) are always
rendered independently at first, starting with a fully-transparent 'black'
backdrop (rgba={0,0,0,0}). The results of this independent composite are then
rendered on top of the group's own backdrop using the group's opacity and
composite mode settings. Conversely non-isolated groups are rendered by
rendering each child layer or sub-stack in turn to the group's backdrop, just as
if there were no stacked group.

* The root stack has a fixed, implicit rendering in OpenRaster: it is to
composite as an isolated group over a background of the application's choice.
* Non-root stacks should be rendered as isolated groups if: a) their `isolation`
property is `isolate` (and not `auto`); or b) their `opacity` is less that 1.0;
or c) they use a `composite-op` other than `svg:src-over`. This inferential
behaviour is intended to provide backwards compatibility with apps which
formerly didn't care about group isolation.

Applications may assume that all stacks are isolated groups if that is all they
support. If they do so, they must declare when writing OpenRaster files that
their layer groups are isolated (`isolation='isolate'`). (Since: 0.0.4)