# Web Application Basics
## Front end, what the user usaully see. Like HTML, CSS and javascript 
- CSS, describes a standard appearance m such as colours, type of text and layout.
- Javascript, enables more complex activity in the web browser. A more advanced type of HTML

## Back end, things you don't see within a webbrowser.
Database
Infrastructure components underpinning web applications. Such as web servers, application server, storage, various networking devices 
WAF, optional for web applicatons. Help filter out dangerous requests away from the web server and provide an element of protection
## URL
Scheme, protocol used to access the website. Most commoon: HTTP and HTTPS
User, Some URLs can include a user’s login details (usually a username) for sites that require authentication.
Host/ Domain, it shows which website you're accessing. Every domain name is unique and registered through domain registrars. Security pov, look for domain names that appear almost like real ones but have small differences (this is called typosquatting). These fake domains are often used in phishing attacks to trick people into giving up sensitive info.
Port, port number help to direct you browser to the right service on the web server
Path, points to the specific file or page on the server you're trying to access
Query String, part of the URL that starts with a question mark (?). It’s often used for things like search terms or form inputs. Since users can modify these query strings, it’s important to handle them securely to prevent attacks like injections, where malicious code could be added.
Fragment start with #, help point to a specific section of a webpage

## HTTP messages 
these messages are packets of data exchanged between a user (the client) and the web server.
There are two types of HTTP messages:

HTTP Requests: Sent by the user to trigger actions on the web application.
HTTP Responses: Sent by the server in response to the user’s request.

each message have a specific format
- Start line, tells you what kind of message is being sent—whether it's a request from the user or a response from the server.
- Headers,  made up of key-value pairs. They give instructions to both the client and the server handling the request or response.
- Empty line, little divider that separates the header from the body. It’s essential because it shows where the headers stop and where the actual content of the message begins.
- body, where the actual data is stored. In a response, it’s where the server puts the content that the user requested (like a webpage or API data).

## HTTP request
http request is what a user sends to a web server to interact with web application and making something happen.
In a request line, there is three parts
HTTP methods, tells the server what action the user wants to perform on the resource identified by the URL path
- GET, Used to fetch data from the server without making any changes
- POST,Sends data to the server, usually to create or update something.
- PUT, Replaces or updates something on the server.
- DELETE, Removes something from the server.
- PATCH, Updates part of a resource. It’s useful for making small changes without replacing the whole thing, but always validate the data to avoid inconsistencies.
- HEAD, Works like GET but only retrieves headers, not the full content. handy for checking metadata without downloading the full response.
- OPTIONS, Tells you what methods are available for a specific resource, helping clients understand what they can do with the server.
- TRACE, it shows which methods are allowed, often for debugging
- CONNECT, Used to create a secure connection, like for HTTPS.
  URL path, tells the server where to find the resource the user is asking for
  in the URL https://tryhackme.com/api/users/123, the path /api/users/123 identifies a specific user
  attackers always try to maniuplate the url path so it is crucial to
  - Validate the URL path to prevent unauthorised access
- Sanitise the path to avoid injection attacks
- Protect sensitive data by conducting privacy and risk assessments
HTTP version
HTTP/0.9 (1991)
The first version, only supported GET requests.

HTTP/1.0 (1996)
Added headers and better support for different types of content, improving caching.

HTTP/1.1 (1997)
Brought persistent connections, chunked transfer encoding, and better caching. It’s still widely used today.

HTTP/2 (2015)
Introduced features like multiplexing, header compression, and prioritisation for faster performance.

HTTP/3 (2022)
Built on HTTP/2, but uses a new protocol (QUIC) for quicker and more secure connections.

Although HTTP/2 and HTTP/3 offer better speed and security, many systems still use HTTP/1.1 because it’s well-supported and works with most existing setups. However, upgrading to HTTP/2 or HTTP/3 can provide significant performance and security improvements as more systems adopt them.

Common Request Headers

Request Header	Example	Description
Host	
Host: tryhackme.com

Specifies the name of the web server the request is for.
User-Agent	
User-Agent: Mozilla/5.0

Shares information about the web browser the request is coming from.
Referer	
Referer: https://www.google.com/

Indicates the URL from which the request came from.
Cookie	
Cookie: user_type=student; room=introtowebapplication; room_status=in_progress

Information the web server previously asked the web browser to store is held in cookies.
Content-Type	
Content-Type: application/json

Describes what type or format of data is in the request.

Request body
- url encoded,A format where data is structured in pairs of key and value where (key=value). Multiple pairs are separated by an (&) symbol
- form data, Allows multiple data blocks to be sent where each block is separated by a boundary string.
- JSON, the data can be sent using the JSON (JavaScript Object Notation) structure
- XML, In XML formatting, data is structured inside labels called tags, which have an opening and closing

## HTTP Response: Status Line and Status Code
Status Line
The first line in every HTTP response is called the Status Line. It gives you three key pieces of info:

HTTP Version: This tells you which version of HTTP is being used.
Status Code: A three-digit number showing the outcome of your request.
Reason Phrase: A short message explaining the status code in human-readable terms.
The Status Code is the number that tells you if the request succeeded or failed, while the Reason Phrase explains what happened. These codes fall into five main categories:

Informational Responses (100-199)
These codes mean the server has received part of the request and is waiting for the rest. It’s a "keep going" signal.

Successful Responses (200-299)
These codes mean everything worked as expected. The server processed the request and sent back the requested data.

Redirection Messages (300-399)
These codes tell you that the resource you requested has moved to a different location, usually providing the new URL.

Client Error Responses (400-499)
These codes indicate a problem with the request. Maybe the URL is wrong, or you’re missing some required info, like authentication.

Server Error Responses (500-599)
These codes mean the server encountered an error while trying to fulfil the request. These are usually server-side issues and not the client’s fault.

Common Status Codes
Here are some of the most frequently seen status codes:

100 (Continue)
The server got the first part of the request and is ready for the rest.

200 (OK)
The request was successful, and the server is sending back the requested resource.

301 (Moved Permanently)
The resource you’re requesting has been permanently moved to a new URL. Use the new URL from now on.

404 (Not Found)
The server couldn’t find the resource at the given URL. Double-check that you’ve got the right address.

500 (Internal Server Error)
Something went wrong on the server’s end, and it couldn’t process your request.
Response headers
Required Response Headers
Some response headers are crucial for making sure the HTTP response works properly. They provide essential info that both the client and server need to process everything correctly. Here are a few important ones:

Date:
Example: Date: Fri, 23 Aug 2024 10:43:21 GMT
This header shows the exact date and time when the response was generated by the server.

Content-Type:
Example: Content-Type: text/html; charset=utf-8
It tells the client what kind of content it’s getting, like whether it’s HTML, JSON, or something else. It also includes the character set (like UTF-8) to help the browser display it properly.

Server:
Example: Server: nginx
This header shows what kind of server software is handling the request. It’s good for debugging, but it can also reveal server information that might be useful for attackers, so many people remove or obscure this one.
Other Common Response Headers
Besides the essential ones, there are other common headers that give additional instructions to the client or browser and help control how the response should be handled.

Set-Cookie:
Example: Set-Cookie: sessionId=38af1337es7a8
This one sends cookies from the server to the client, which the client then stores and sends back with future requests. To keep things secure, make sure cookies are set with the HttpOnly flag (so they can’t be accessed by JavaScript) and the Secure flag (so they’re only sent over HTTPS).

Cache-Control:
Example: Cache-Control: max-age=600
This header tells the client how long it can cache the response before checking with the server again. It can also prevent sensitive info from being cached if needed (using no-cache).

Location:
Example: Location: /index.html
This one’s used in redirection (3xx) responses. It tells the client where to go next if the resource has moved. If users can modify this header during requests, be careful to validate and sanitise it—otherwise, you could end up with open redirect vulnerabilities, where attackers can redirect users to harmful sites.

Security Headers
HTTP Security Headers help improve the overall security of the web application by providing mitigations against attacks like Cross-Site Scripting (XSS)
CSP, Malicious code could be hosted on a separate website or domain and injected into the vulnerable website. A CSP provides a way for administrators to say what domains or sources are considered safe and provides a layer of mitigation to such attacks.

We see the use of:

default-src
- which specifies the default policy of self, which means only the current website.

script-src
- which specifics the policy for where scripts can be loaded from, which is self along with scripts hosted on https://cdn.tryhackme.com

style-src
- which specifies the policy for where style CSS style sheets can be loaded from the current website (self)
HSTS
The HSTS header ensures that web browsers will always connect over HTTPS
Here’s a breakdown of the example HSTS header by directive:

max-age
- This is the expiry time in seconds for this setting

includeSubDomains
- An optional setting that instructs the browser to also apply this setting to all subdomains.

preload
- This optional setting allows the website to be included in preload lists. Browsers can use preload lists to enforce HSTS before even having their first visit to a website.
X-Content-Type-Options
nosniff
- This directive instructs the browser not to sniff or guess the MIME type.
  Referrer-Policy
  Referrer-Policy: no-referrer
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin
no-referrer
- This completely disables any information being sent about the referrer

same-origin
- This policy will only send referrer information when the destination is part of the same origin. This is helpful when you want referrer information passed when hyperlinks are within the same website but not outside to external websites.

strict-origin
- This policy only sends the referrer as the origin when the protocol stays the same. So, a referrer is sent when an HTTPS connection goes to another HTTPS connection.

strict-origin-when-cross-origin
- This is similar to strict-origin except for same-origin requests, where it sends the full URL path in the origin header.
