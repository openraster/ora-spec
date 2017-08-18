# Layer Edit Locking Status

Editor applications sometimes have a concept of layers which are currently
locked to prevent accidental editing. This state should be saved as part of
OpenRaster files which might be opened again for editing in order to assist user
workflow. Therefore, the `<layer/>` element in a layer stack definition should
support the following extra attribute:

* `"edit-locked"`: the layer edit locking status, expressed as an
[xsd:boolean](http://www.w3.org/TR/xmlschema-2/#boolean). True values (`"true"`,
canonically) mean that the layer was locked to prevent accidental edits by the
user, and should be regarded as un-editable after loading if such immutable
layers are supported by the editor. False values (`"false"`, canonically)
indicate that the layer is not edit locked, and should be treated as editable
immediately after loading. The default is false. (It is intended that other
kinds of "locking", e.g. alpha locking which makes the alpha channel of a layer
immutable, should suffix their attribute names with the string `"-locked"` for
consistency. Don't just use `"locked"` on its own; there's more than one kind of
immutability.)

## Example

When a user does not wish to accidentally paint colour onto a monochrome sketch
layer, they may wish to edit-lock that layer in the user interface. When the
working document is saved, such a layer stack might be saved as:

	<stack>
		<layer name="sketch" x="0" y="0" src="data/layer001.png" edit-locked="true"/>
		<layer name="colours" x="-512" y="-128" src="data/layer002.png"/>
	</stack>

## Impact on Applications

This change has no impact on the viewing baseline intent.

Applications supporting the full baseline intent which permit layers to be made
immutable by the user to prevent accidental edits should mark edit-locked layers
with the edit-locked attribute when saving, and restore their edit-locked state
when loading. Applications may impose their own definition of what edit-locked
actually means; for example, in MyPaint, layers marked as locked in this way
cannot be painted to, but are also unavailable for selection by brushstroke or
for brushstroke picking.

The edit-locked attribute must be omitted when saving for the archival intent.

## Support

Implemented in Krita and MyPaint development trunks since 2012-03-26.