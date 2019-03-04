
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

##### disable automatic encoding

You may **disable** the **automatic encoding** of a body as long as the body object is a **byte** array:
```dart
final jsonBytes = utf8.encode(json.encode({"key": "value"}));
final response = Response.ok(jsonBytes)..encodeBody = false;
```
See a later section for more details on content type to codec mappings. Also, see the documentation for  `CodecRegistry`  for details on built-in codecs and adding codecs.


### Streaming Response Bodies

A **body** object **may also be a  `Stream<T>`**.  `Stream<T>`  body objects are most **often used when serving files**. This allows the contents of the file to be **streamed from disk** to the HTTP client **without** having to load the whole file into memory first. (See also  `FileController`.)
```dart
final imageFile = File("image.jpg");
final imageByteStream = imageFile.openRead();
final response = new Response.ok(imageByteStream)
  ..contentType = new ContentType("image", "jpeg");
```
> ⚡ When a body object is a  `Stream<T>`, the response will not be sent until the stream is closed. 

> For finite streams - like those from opened filed - this happens as soon as the entire file is read. For streams that you construct yourself, you must close the stream some time after the response has been returned.

## Codecs and Content Types

In the above sections, we glossed over how a codec gets selected when preparing the response body. The common case of  `ManagedObject<T>`  body objects that are sent as UTF8 encoded JSON 'just works' and is suitable for most applications. When serving assets for a web application or different data formats like XML, it becomes important to understand how Aqueduct's codec registry works.

> ⚡ `CodecRegistry`  contains mappings from content types to  `Codec`s. These codecs **encode response** bodies and **decode request** bodies. 
There are three built-in codecs for 
  - `application/json`, 
  - `application/x-www-form-urlencoded`  and  
  - `text/*`. 

> ⚡ When a response is being sent, the repository is searched for an entry that exactly matches the primary and subtype of the  `Response.contentType`. If an entry exists, the associated  `Codec`  starts the conversion. 

For example, if the content type is  `application/json; charset=utf-8`, the built-in  `application/json`  codec encodes the body object.

> ⚡ If there isn't an exact match, but there is an entry for the primary type with the wildcard (`*`) subtype, that codec is used. For example, the built-in codec for  `text/*`will be selected for both  `text/plain`  and  `text/html`. 

### register codecs into CodeRegistery

If there was something special that had to be done for  `text/html`, a more specific codec may be added for that type:
```dart
class MyChannel extends ApplicationChannel {
  @override
  Future prepare() async {
    CodecRegistry.defaultInstance.add(ContentType("application", "html"), HTMLCodec());
  }
}
```
> ⚡ Codecs must be added in your  `ApplicationChannel.prepare`  method. **The codec must implement  `Codec`  from  `dart:convert`**. In the above example, when a response's content type is  `text/html`, the  `HTMLCodec`  will encode the body object. This codec takes precedence over  `text/*`  because it is more specific.

> 📓 When selecting a codec for a response body, the  `ContentType.charset`  doesn't impact which codec is selected. 

If a response's content-type has a charset, then a charset encoder like  `UTF8`  will be applied as a last encoding step. For example, a response with content-type  `application/json; charset=utf-8`  will encode the body object as a JSON string, which is then encoded as a list of UTF8 bytes. It is required that a response body's eventually encoded type is a list of bytes, so it follows that a codec that produces a string must have a charset.

> ⚡ If there is no codec in the repository for the content type of a  `Response`, the body object must be a  `List<int>`  or  `Stream<List<int>>`. 
>  📘 If you find yourself converting data prior to setting it as a body object, it may make sense to add your own codec to  `CodecRegistry`.

> ⚡ A request's body always starts as a **list of bytes** and is decoded into Dart objects. To decode a JSON request body, it first must be decoded from the **list of UTF8** bytes into a **string**. 

#### register JsonCodec into CodecRegistry

It is possible that a client could omit the charset in its content-type header. Codecs added to  `CodecRegistry`  may specify a default charset to interpret a charset-less content-type. When a codec is added to the repository, if content-type's charset is non-null, that is the default. For example, the JSON codec is added like this:
```dart
CodecRegistry.defaultInstance.add(
  ContentType("application", "json", charset: "utf-8"),
  const JsonCodec(),
  allowCompression: true);
```
If no charset is specified when registering a codec, no charset decoding occurs on a request body if one  doesn't exist. Content-types that are decoded from a  `String`should not use a default charset because the repository would always attempt to decode the body as a string first.

### Compression with gzip

Body objects may be compressed with  `gzip`  if the HTTP client allows it  _and_  the  `CodecRegistry`  has been configured to compress the content type of the response. 

> ⚡ The three built-in codecs 
> -  `application/json`
> -   `application/x-www-form-urlencoded`  and  
> - `text/*`  
are all configured to allow compression. Compression occurs as the last step of conversion and only if the HTTP client sends the  `Accept-Encoding: gzip`  header.

Content types that are not in the codec repository will not trigger compression, even if the HTTP client allows compression with the  `Accept-Encoding`  header. This is to prevent binary contents like images from being 'compressed', since they are likely already compressed by a content-specific algorithm. 

> ⚡ In order for Aqueduct to compress a content type other than the built-in types, you may add a codec to the repository with the  `allowCompression`  flag. (The default value is  `true`.)
```dart
CodecRegistry.add(
  ContentType("application", "x-special"),
   MyCodec(),
  allowCompression: true);
```
You may also set whether or not a content type uses compression without having to specify a codec if no conversion step needs to occur:
```dart
CodecRegistry.setAllowsCompression(new ContentType("application", "x-special"), true);
```

## Serializable Objects

Encoding and decoding primarily takes or yields simple Dart objects like  `Map`,  `List`and  `String`. It is often beneficial to add structure to body objects for use in your code. Classes that implement  `Serializable`  provide this structure. A  `Serializable`  object must implement a  `readFromMap()`  and  `asMap()`  method to convert their structure into or from a Dart  `Map`.

> ⚡ An object that extends  `Serializable`  may be used as a response body object directly:

```dart
class Person extends Serializable {
  String name;
  String email;
  Map<String, dynamic> asMap() {
    return {
      "name": name,
      "email": email
    };
  }
  void readFromMap(Map<String, dynamic> inputMap) {
    name = inputMap['name'];
    email = inputMap['email'];
  }
}

final person = Person();
final response = Response.ok(person);
```

When responding with a  `Serializable`, its  **`asMap()`**  is called **prior** to any **encoding** by the codec registry.  `ManagedObject<T>`, part of the Aqueduct ORM, implements  `Serializable`  so results from  `Query<T>`  may be body objects:
```dart
final query = Query<Person>(context)..where((p) => p.id).equalTo(1);
final person = await query.fetchOne();
final response = Response.ok(person);
```
A response body object can also be a list of  `Serializable`  objects.
```dart
final query = Query<Person>(context);
final people = await query.fetch();
final response = Response.ok(people);
```
The entire flow of a body object is shown in the following diagram. Each orange item is an allowed body object type and shows the steps it will go through when being encoded to the HTTP response body. For example, a  `Serializable`  goes through three steps, whereas a  `List<int>`  goes through zero steps and is added as-is to the HTTP response.

![Response Body Object Flow](https://aqueduct.io/docs/img/response_flow.png)

A serializable object can be read from a request body:

final person = Person()..readFromMap(await request.body.decode());

Serializable objects are the only types of objects that can be  [bound to a ResourceController argument](https://aqueduct.io/docs/http/resource_controller/).
```dart
@Operation.post()
Future<Response> addPerson(@Bind.body() Person person) async {
  final insertedPerson = await context.insertObject(person);
  return Response.ok(insertedPerson);
}
```
### Serializable and OpenAPI Generation

See the section on how  `Serializable`  types work with OpenAPI documentation generation  [here](https://aqueduct.io/docs/openapi/components/).

### Example: multipart/form-data

Add  `package:mime`  to your  `pubspec.yaml`  as a dependency.

dependencies:
  mime: any # prefer a better constraint than this

And then decode the body with the objects from that package:
```dart
import 'package:aqueduct/aqueduct.dart';
import 'package:mime/mime.dart';

class MyController extends ResourceController {
  MyController() {
    acceptedContentTypes = [ContentType("multipart", "form-data")];
  }

  @Operation.post()
  Future<Response> postForm() async {}
    final transformer = MimeMultipartTransformer(request.raw.headers.contentType.parameters["boundary"]);
    final bodyStream = Stream.fromIterable([await request.body.decode<List<int>>()]);
    final parts = await transformer.bind(bodyStream).toList();

    for (var part in parts) {
      final headers = part.headers;
      final content = await part.toList();
      ...
    }    
  }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NjIxNDMxMTksNjY5NTk0NzYwLDE3ND
QwNjc2NzQsOTU5MTUwMTQ4LC00MjEyNTc1MzQsLTEzNjMzMjEw
MjQsMjEwMjMwNzM2OF19
-->