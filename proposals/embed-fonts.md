# Embed Fonts as TTF Files

*Status*: proposal

*Author*: Marcos

*Submission date*: 28 May, 2010

*Implementations*: none

To improve the user experience and maximize the representation accuracy is
possible to store the `.ttf` fonts inside the `.ora` files.

Additionally the implementation must check if the font is libre, then:

* If it is libre: embed it directly into the .ora
* If it is not libre: alert the user about the license issues, and offer to
avoid the embedding or to embed it despite of the legal issues.
