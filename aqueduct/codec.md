 Not all content types require two conversion steps. For example, when serving an HTML file, the body object is already an HTML  `String`. It will only be converted by a charset encoder:
```dart
var html = "<html></html>";
var response = Response.ok(html)
  ..contentType = ContentType.html;
```
#### Image Codec
An image body object needs no conversion at all, since it is already a list of bytes. **If** there is **no registered codec** for a content-type, the body object **must be a byte array** (`List<int>`  where each value is between 0-255).
```dart
final imageFile = File("image.jpg");
final imageBytes = await imageFile.readAsBytes();
final response = Response.ok(imageBytes)
  ..contentType = ContentType("image", "jpeg");
```
You may disable the automatic encoding of a body as long as the body object is a byte array:
```dart
final jsonBytes = utf8.encode(json.encode({"key": "value"}));
final response = Response.ok(jsonBytes)..encodeBody = false;
```
See a later section for more details on content type to codec mappings. Also, see the documentation for  `CodecRegistry`  for details on built-in codecs and adding codecs.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgxNTA3MjQ2MSwyMTAyMzA3MzY4XX0=
-->