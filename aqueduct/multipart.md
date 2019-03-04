
### Multipart header
#### [What should a Multipart HTTP request with multiple files look like? [duplicate]](https://stackoverflow.com/questions/913626/what-should-a-multipart-http-request-with-multiple-files-look-like)
```html
>   
> when should we use it

Quentin's answer is right: use  `multipart/form-data`  if the form contains a file upload, and  `application/x-www-form-urlencoded`  otherwise, which is the default if you omit  `enctype`.

I'm going to:

-   add some more HTML5 references
-   explain  **why**  he is right with a form submit example

## HTML5 references

There are  [three possibilities](https://www.w3.org/TR/html5/sec-forms.html#element-attrdef-form-enctype)  for  `enctype`:

-   [`application/x-www-form-urlencoded`](https://www.w3.org/TR/html5/sec-forms.html#urlencoded-form-data)
-   [`multipart/form-data`](https://www.w3.org/TR/html5/sec-forms.html#multipart-form-data)  (spec points to  [RFC7578](https://tools.ietf.org/html/rfc7578))
-   [`text/plain`](https://www.w3.org/TR/html5/sec-forms.html#plain-text-form-data). This is "not reliably interpretable by computer", so it should never be used in production, and we will not look further into it.

## How to generate the examples

Once you see an example of each method, it becomes obvious how they work, and when you should use each one.

You can produce examples using:

-   `nc -l`  or an ECHO server:  [HTTP test server accepting GET/POST requests](https://stackoverflow.com/questions/5725430/http-test-server-accepting-get-post-requests/52351480#52351480)
-   an user agent like a browser or cURL

Save the form to a minimal  `.html`  file:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <title>upload</title>
</head>
<body>
<form action="http://localhost:8000" method="post" enctype="multipart/form-data">
  <p><input type="text" name="text1" value="text default">
  <p><input type="text" name="text2" value="a&#x03C9;b">
  <p><input type="file" name="file1">
  <p><input type="file" name="file2">
  <p><input type="file" name="file3">
  <p><button type="submit">Submit</button>
</form>
</body>
</html>
```

We set the default text value to  `a&#x03C9;b`, which means  `aωb`  because  `ω`is  `U+03C9`, which are the bytes  `61 CF 89 62`  in UTF-8.

Create files to upload:

```
echo 'Content of a.txt.' > a.txt

echo '<!DOCTYPE html><title>Content of a.html.</title>' > a.html

# Binary file containing 4 bytes: 'a', 1, 2 and 'b'.
printf 'a\xCF\x89b' > binary
```

Run our little echo server:

```
while true; do printf '' | nc -l 8000 localhost; done
```

Open the HTML on your browser, select the files and click on submit and check the terminal.

`nc`  prints the request received.

Tested on: Ubuntu 14.04.3,  `nc`  BSD 1.105, Firefox 40.

## multipart/form-data


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
eyJoaXN0b3J5IjpbOTEyNDEzMDY2LDE2OTA4NzkzNTMsLTg3ND
kzODI2M119
-->