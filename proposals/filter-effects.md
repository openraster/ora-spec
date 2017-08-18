# Filter Effects

*Status*: proposal (currently unimplemented?)

*Author*: unknown

*Submission date*: 18 May, 2013

*Implementations*: unknown

*Note*: this is an early draft of the specification, do not take anything on
this page for granted, pretty much everything is open for discussion.

As a standard extension, outside baseline but defined in the standard schema,
OpenRaster layers stacks should be able to contain parameterised filters which
themselves might contain sub-stacks. This document shows how the extension would
be integrated into baseline OpenRaster stack.xml syntax, describes the
additional elements and attributes, and lists the available filters.

	<stack>
		<stack>
			<filter name="invert" type="invert" />
			<!-- layers -->
		</stack>
		<!-- ... layers ... -->
		<filter name="filter1" type="standard:gaussianblur">
			<params>
				<param name="radius">10</param>
			</params>
			<stack>
				<layer name="mask2" src="data/mask2.data" composite-op="svg:src-over" />
    			<layer name="mask1" src="data/mask1.png" />
			</stack>
		</filter>
		<!-- ... more layers ... -->
	</stack>

## Semantics

TODO: define how flters are to be applied. Presumably to just the layers inside
the nested `<stack/>` element? If so, how is the result to be composited onto
the results of the layers below?

### filter element

This element defines a layer which instead of containing pixel data, applies a
filter onto the composited result of the layers below it in the stack.
The attributes are:

* `"type"`: the type of the filter. Type names make use of namespaces, they must
have the following form: "namespace:name". Three namespaces are defined:
  * `"standard"`: for the list of filters and the associated mathematics, see
  the relevant OpenRaster specification
  * `"composite"`: composite filter are filters which are a composition of a
  list of standard filters. The name must be the name of a filter defined in the
  same file.
  * `"application"`: non-standard filters; the name is formed as followed
  "application:filtername". For example, for an application called MyGraphApp,
  the full name is "application:MyGraphApp:MyFilter". Use of such a type of
  filter prevents the file from being a Full Baseline OpenRaster file; however
  use of the `"output"` attribute defined below allows the file to be conformant
  to the Viewing Baseline.
* `"output"`: the file name of the data of the filter output, this attribute is
only needed when the type of the filter is of the "application" namespace, and
if the user wants to create a Viewing Baseline layer stack.

### params element

This element is used to describe the parameters used for a filter layer. The
standard filters each have a defined list of parameters, defined permissible
values. Attributes:

* `"version"`: correspond to the version of the filter, either the version of
the filter specification for standard filter, or any number defined by the
application for application-specific filters.

### param element

Description of the attributes:

* `"name"`: the name of the parameter. The parameter's value is formed by the
content of the param element.

For "composite" filters, the `params` element allows variables to be defined
which are then used to parameterise each contained filter. For example, consider
a composite function composed of `"standard:blur"` filter with the parameter
`"radius"` applied with a `"standard:invert"` filter. It is defined as follows
in the composite section:

	<compositefilter name="blurinvert" >
		<filter name="blur">
			<params>
				<param name="radius">
					@R@
				</param>
			</params>
		</filter>
		<filter name="invert" />
	</compositefilter>

Then the usage would be:

	<filter name="composite:blurinvert" >
		<params>
			<param name="R">10</param>
		</params>
	</filter>

TODO: reconsider this, is it useful ? it just come out of my mind that is the
only usefull use of parameters for composite filter, but if it's useless, let's
not allow any parameters for them!

## Standard Filters

The mathematics behind each filter is described in the SVG 1.1 Specification,
and each filter accepts the same parameters.

OpenRaster files may use filters not described in this document, in which case
the file won't be valid for interoperability, unless the result of the operation
is also available (see the output attribute above).

### ColorMatrix

See [SVG 1.1 Specification](http://www.w3.org/TR/SVG11/filters.html#feColorMatrixElement).

### ConvolveMatrix

See [SVG 1.1 Specification](http://www.w3.org/TR/SVG11/filters.html#feConvolveMatrixElement).

### ComponentTransfer

See [SVG 1.1 Specification](http://www.w3.org/TR/SVG11/filters.html#feComponentTransferElement).

### GaussianBlur

See [SVG 1.1 Specification](http://www.w3.org/TR/SVG11/filters.html#feGaussianBlurElement).

### Tile

See [SVG 1.1 Specification](http://www.w3.org/TR/SVG11/filters.html#feTileElement).

### Morphology

See [SVG 1.1 Specification](http://www.w3.org/TR/SVG11/filters.html#feMorphologyElement).

### Distortion correction

### Burn and Dodge