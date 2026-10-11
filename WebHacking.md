#Web Application Basics
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
  
- 
