
### Multipart header
#### [What should a Multipart HTTP request with multiple files look like? [duplicate]](https://stackoverflow.com/questions/913626/what-should-a-multipart-http-request-with-multiple-files-look-like)

`enctype='multipart/form-data`  is an encoding type that allows files to be sent through a  _POST_. Quite simply, without this encoding the files cannot be sent through  _POST_.

If you want to allow a user to upload a file via a form, you must use this  _enctype_.

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
eyJoaXN0b3J5IjpbMTg3MDgzMzQwNSwxNjkwODc5MzUzLC04Nz
Q5MzgyNjNdfQ==
-->