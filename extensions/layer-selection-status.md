# Layer Selection Status

Editor applications normally have a concept of which layer or layers, are
currently selected by the user for editing. It is more convenient for the user
if such applications save this information to OpenRaster files, and are able to
select the same layer or layers again when the file is reloaded. Therefore, the
`<layer/>` element in a layer stack definition should support the following
extra attribute:

* `"selected"`: the layer selection status, expressed as an
[xsd:boolean](http://www.w3.org/TR/xmlschema-2/#boolean). True values (`"true"`,
canonically), mean that the layer is selected by the user, and may be made
active for editing after loading. False values (`"false"`, canonically) indicate
that the layer is not selected. The default is false.

## Example

	<stack>
		<layer name="sketch" x="0" y="0" src="data/layer001.png"/>
		<layer name="colours" x="-512" y="-128" src="data/layer002.png" selected="true"/>
	</stack>

## Impact on Applications

This change has no impact on the viewing baseline intent.

Apps supporting the full baseline intent which permit layers to be chosen by the
user for editing or any other purpose should mark a single such chosen layer as
`selected` when saving, ideally a layer currently receptive to editing. Upon
loading, apps should respect the user selection and should make any layer marked
as `selected` both selected in the interface and, if supported, receptive to
editing. When loading, it is acceptable to make only a single layer selected
(and possibly receptive to editing) if that is all that is supported and
multiple layers are marked as `selected` in the OpenRaster file being loaded.
The choice of which layer to make active is left as an implementation detail,
but should be guided by the `selected` attributes of layers.

The selected attribute must be omitted when saving for the archival intent.

## Support

Implemented in Krita and MyPaint development trunks since 2012-03-26.
