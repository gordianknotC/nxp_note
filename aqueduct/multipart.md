
### Multipart header
#### [What should a Multipart HTTP request with multiple files look like? [duplicate]](https://stackoverflow.com/questions/913626/what-should-a-multipart-http-request-with-multiple-files-look-like)

There are  [three possibilities](https://www.w3.org/TR/html5/sec-forms.html#element-attrdef-form-enctype)  for  `enctype`:

-   [`application/x-www-form-urlencoded`](https://www.w3.org/TR/html5/sec-forms.html#urlencoded-form-data)
-   [`multipart/form-data`](https://www.w3.org/TR/html5/sec-forms.html#multipart-form-data)  (spec points to  [RFC7578](https://tools.ietf.org/html/rfc7578))
-   [`text/plain`](https://www.w3.org/TR/html5/sec-forms.html#plain-text-form-data). This is "not reliably interpretable by computer", so it should never be used in production, and we will not look further into it.

When you are writing client-side code:

-   use  `multipart/form-data`  when your form includes any  `<input type="file">`  elements
-   otherwise you can use  `multipart/form-data`  or  `application/x-www-form-urlencoded`  but  `application/x-www-form-urlencoded`  will be more efficient

When you are writing server-side code:

-   Use a prewritten form handling library
- 
`enctype='multipart/form-data`  is an encoding type that allows files to be sent through a  _POST_. Quite simply, without this encoding the files cannot be sent through  _POST_. If you want to allow a user to upload a file via a form, you must use this  _enctype_.

```http
POST / HTTP/1.1
[[ Less interesting headers ... ]]
Content-Type: multipart/form-data; boundary=---------------------------735323031399963166993862150
Content-Length: 834

-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="text1"

text default
-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="text2"

aωb
-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="file1"; filename="a.txt"
Content-Type: text/plain

Content of a.txt.

-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="file2"; filename="a.html"
Content-Type: text/html

<!DOCTYPE html><title>Content of a.html.</title>

-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="file3"; filename="binary"
Content-Type: application/octet-stream

aωb
-----------------------------735323031399963166993862150--

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbODgxOTQxNTE0LDE2OTA4NzkzNTMsLTg3ND
kzODI2M119
-->