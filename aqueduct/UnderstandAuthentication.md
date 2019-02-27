following material fetched from [here](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html)

 ## Understanding Login Authentication

When you try to access a protected web resource, the web container activates the authentication mechanism that has been configured for that resource. You can specify the following authentication mechanisms:

-   HTTP basic authentication
-   Form-based login authentication
-   Client certificate authentication
-   Mutual authentication
-   Digest authentication

If you do not specify one of these mechanisms, the user will not be authenticated.

### Using HTTP Basic Authentication

[Figure 32-2](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483341)  shows what happens if you specify  _HTTP basic authentication_.

![HTTP Basic Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-httpBasicAuthentication.gif)

Figure 32-2 HTTP Basic Authentication

With basic authentication, the following things occur:

-   A client requests access to a protected resource.
-   The web server returns a dialog box that requests the user name and password.
-   The client submits the user name and password to the server.
-   The server validates the credentials and, if successful, returns the requested resource.

HTTP basic authentication is **not** particularly secure. Basic authentication sends user names and passwords over the Internet as text that is uu-encoded (Unix-to-Unix encoded) but not encrypted. This form of authentication, which **uses Base64 encoding**, can expose your user names and passwords **unless all connections are over SSL**. If someone can intercept the transmission, the user name and password information can easily be decoded.

[Example: Basic Authentication with JAX-RPC](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security7.html#wp156943)  is an example application that uses HTTP basic authentication in a JAX-RPC service.

### Using Form-Based Authentication

[Figure 32-3](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483393)  shows what happens if you specify  _form-based authentication_, in which you can customize the login screen and error pages that an HTTP browser presents to the end user.

![Form-Based Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-formBasedLogin.gif)

Figure 32-3 Form-Based Authentication

With form-based authentication, the following things occur:

-   A client requests access to a protected resource.
-   If the client is unauthenticated, the server redirects the client to a login page.
-   The client submits the login form to the server.
-   If the login succeeds, the server redirects the client to the resource. If the login fails, the client is redirected to an error page.

Form-based authentication is **not** particularly secure. In form-based authentication, the content of the user dialog box is sent as plain text, and the target server is not authenticated. This form of authentication can expose your user names and passwords **unless all connections are over SSL**. If someone can intercept the transmission, the user name and password information can easily be decoded.

[Example: Using Form-Based Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp303355)  is an example application that uses form-based authentication.



### Using Client-Certificate Authentication

Client-certificate authentication is a more secure method of authentication than either basic or form-based authentication. It **uses HTTP over SSL**, in which the server and, optionally, the client **authenticate one another** using public key certificates.  Secure Socket Layer  (SSL) provides data encryption, server authentication, message integrity, and optional client authentication for a TCP/IP connection. **You can think of a  public key certificate  as the digital equivalent of a passport**. It is issued by a trusted organization, which is called a  certificate authority  (CA), and provides identification for the **bearer**.

If you specify  client-certificate authentication, the web server will authenticate the client using the client's  X.509 certificate, a public key certificate that conforms to a standard that is defined by X.509 Public Key Infrastructure (PKI). Before running an application that uses SSL, you must configure SSL support on the server (see  [Installing and Configuring SSL Support](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security6.html#wp509934)) and set up the public key certificate (see  [Understanding Digital Certificates](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security6.html#wp80737)).

[Example: Client-Certificate Authentication over HTTP/SSL with JAX-RPC](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security7.html#wp498398)  describes an example application that uses client-certificate authentication.



### Using Mutual Authentication

With  _mutual authentication_ , the server and the client authenticate each other. There are two types of mutual authentication:

-   **Certificate-based** mutual authentication (see  [Figure 32-4](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp488092))
-   **User name- and password-based** mutual authentication (see  [Figure 32-5](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483607))


[Figure 32-4](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp488092)  shows what occurs during certificate-based mutual authentication.

![Certificate-Based Mutual Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-sslBasedMutualAuthenticationWithCertificates.gif)

⚡ Figure 32-4 **Certificate-Based** Mutual Authentication

In certificate-based mutual authentication, the following things occur:

-   A client requests access to a protected resource.
-   The web server presents its certificate to the client.
-   The client verifies the server's certificate.
-   If successful, the client **sends its certificate** to the server.
-   The server verifies the client's credentials.
-   If successful, the server grants access to the protected resource requested by the client.

[Example: Client-Certificate Authentication over HTTP/SSL with JAX-RPC](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security7.html#wp498398)  describes an example application that uses certificate-based mutual authentication.

[Figure 32-5](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483607)  shows what occurs during user name- and password-based mutual authentication.

![Username/Password-Based Mutual Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-sslBasedMutualAuthenticationWithUsernamePassword.gif)

Figure 32-5 User Name- and Password-Based Mutual Authentication

In user name- and password-based mutual authentication, the following things occur:

-   A client requests access to a protected resource.
-   The web server presents its certificate to the client.
-   The client verifies the server's certificate.
-   If successful, the client sends **its user name and password** to the server, which verifies the client's credentials.
-   If the verification is successful, the server grants access to the protected resource requested by the client.



<!--stackedit_data:
eyJoaXN0b3J5IjpbNTIxODI2MzYsMTYxODY3MzUwNCwxNzY2Mz
Q3NTk4LC0zMTM3MjY0OTgsOTIwMjA0OTY2XX0=
-->