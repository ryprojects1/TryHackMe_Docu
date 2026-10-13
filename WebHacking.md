# Web Application Basics

## Front End

What the user usually sees. Built with HTML, CSS, and JavaScript.

- **CSS**: Describes standard appearance such as colors, type of text, and layout.
- **JavaScript**: Enables more complex activity in the web browser. A more advanced type of HTML.

## Back End

Things you don't see within a web browser.

- **Database**: Stores and manages application data.
- **Infrastructure**: Components underpinning web applications, such as web servers, application servers, storage, and various networking devices.
- **WAF (Web Application Firewall)**: Optional for web applications. Helps filter out dangerous requests away from the web server and provides an element of protection.

---

## URL Structure

A URL (Uniform Resource Locator) has several components:

### Scheme
Protocol used to access the website. Most common: HTTP and HTTPS.

### User
Some URLs can include a user's login details (usually a username) for sites that require authentication.

### Host / Domain
Shows which website you're accessing. Every domain name is unique and registered through domain registrars.

**Security perspective**: Look for domain names that appear almost like real ones but have small differences (typosquatting). These fake domains are often used in phishing attacks to trick people into giving up sensitive information.

### Port
Port numbers help direct your browser to the right service on the web server.

### Path
Points to the specific file or page on the server you're trying to access.

### Query String
Part of the URL that starts with a question mark (?). Often used for search terms or form inputs. Since users can modify these query strings, it's important to handle them securely to prevent attacks like injections, where malicious code could be added.

### Fragment
Starts with #. Helps point to a specific section of a webpage.

---

## HTTP Messages

HTTP messages are packets of data exchanged between a user (the client) and the web server.

There are two types of HTTP messages:

- **HTTP Requests**: Sent by the user to trigger actions on the web application.
- **HTTP Responses**: Sent by the server in response to the user's request.

### Message Structure

Each message has a specific format:

- **Start line**: Tells you what kind of message is being sent (request from user or response from server).
- **Headers**: Made up of key-value pairs. They give instructions to both the client and the server.
- **Empty line**: Separates the header from the body. Essential because it shows where headers stop and content begins.
- **Body**: Where the actual data is stored. In a response, it's where the server puts the content that the user requested (like a webpage or API data).

---

## HTTP Request

An HTTP request is what a user sends to a web server to interact with a web application and make something happen.

### Request Line Components

The request line has three parts:

### HTTP Methods

Tells the server what action the user wants to perform on the resource identified by the URL path.

- **GET**: Used to fetch data from the server without making any changes.
- **POST**: Sends data to the server, usually to create or update something.
- **PUT**: Replaces or updates something on the server.
- **DELETE**: Removes something from the server.
- **PATCH**: Updates part of a resource. Useful for making small changes without replacing the whole thing. Always validate data to avoid inconsistencies.
- **HEAD**: Works like GET but only retrieves headers, not the full content. Handy for checking metadata without downloading the full response.
- **OPTIONS**: Tells you what methods are available for a specific resource, helping clients understand what they can do with the server.
- **TRACE**: Shows which methods are allowed, often for debugging.
- **CONNECT**: Used to create a secure connection, like for HTTPS.

### URL Path

Tells the server where to find the resource the user is asking for.

Example: In `https://tryhackme.com/api/users/123`, the path `/api/users/123` identifies a specific user.

**Attackers always try to manipulate the URL path**, so it is crucial to:
- Validate the URL path to prevent unauthorized access.
- Sanitize the path to avoid injection attacks.
- Protect sensitive data by conducting privacy and risk assessments.

### HTTP Version

- **HTTP/0.9 (1991)**: The first version, only supported GET requests.
- **HTTP/1.0 (1996)**: Added headers and better support for different types of content, improving caching.
- **HTTP/1.1 (1997)**: Brought persistent connections, chunked transfer encoding, and better caching. Still widely used today.
- **HTTP/2 (2015)**: Introduced multiplexing, header compression, and prioritization for faster performance.
- **HTTP/3 (2022)**: Built on HTTP/2, uses a new protocol (QUIC) for quicker and more secure connections.

Although HTTP/2 and HTTP/3 offer better speed and security, many systems still use HTTP/1.1 because it's well-supported and works with most existing setups. Upgrading to HTTP/2 or HTTP/3 can provide significant performance and security improvements.

### Common Request Headers

| Header | Example | Description |
|--------|---------|-------------|
| **Host** | `Host: tryhackme.com` | Specifies the name of the web server the request is for. |
| **User-Agent** | `User-Agent: Mozilla/5.0` | Shares information about the web browser the request is coming from. |
| **Referer** | `Referer: https://www.google.com/` | Indicates the URL from which the request came from. |
| **Cookie** | `Cookie: user_type=student; room=introtowebapplication` | Information the web server previously asked the web browser to store. |
| **Content-Type** | `Content-Type: application/json` | Describes what type or format of data is in the request. |

### Request Body

Data sent in the request body can be formatted in several ways:

- **URL Encoded**: Data structured in pairs of key and value (key=value). Multiple pairs are separated by an (&) symbol.
- **Form Data**: Allows multiple data blocks where each block is separated by a boundary string.
- **JSON**: Data sent using the JSON (JavaScript Object Notation) structure.
- **XML**: Data structured inside labels called tags, which have opening and closing elements.

---

## HTTP Response: Status Line and Status Code

### Status Line

The first line in every HTTP response is called the Status Line. It gives you three key pieces of information:

- **HTTP Version**: Which version of HTTP is being used.
- **Status Code**: A three-digit number showing the outcome of your request.
- **Reason Phrase**: A short message explaining the status code in human-readable terms.

### Status Code Categories

Status codes fall into five main categories:

#### Informational Responses (100-199)
These codes mean the server has received part of the request and is waiting for the rest. It's a "keep going" signal.

#### Successful Responses (200-299)
Everything worked as expected. The server processed the request and sent back the requested data.

#### Redirection Messages (300-399)
The resource you requested has moved to a different location, usually providing the new URL.

#### Client Error Responses (400-499)
Problem with the request. Maybe the URL is wrong, or you're missing required info, like authentication.

#### Server Error Responses (500-599)
The server encountered an error while trying to fulfill the request. Usually server-side issues, not the client's fault.

### Common Status Codes

- **100 (Continue)**: The server got the first part of the request and is ready for the rest.
- **200 (OK)**: The request was successful, and the server is sending back the requested resource.
- **301 (Moved Permanently)**: The resource has been permanently moved to a new URL. Use the new URL from now on.
- **404 (Not Found)**: The server couldn't find the resource at the given URL. Check the address.
- **500 (Internal Server Error)**: Something went wrong on the server's end and it couldn't process the request.

---

## Response Headers

### Required Response Headers

Some response headers are crucial for making sure the HTTP response works properly.

#### Date
Example: `Date: Fri, 23 Aug 2024 10:43:21 GMT`

Shows the exact date and time when the response was generated by the server.

#### Content-Type
Example: `Content-Type: text/html; charset=utf-8`

Tells the client what kind of content it's getting (HTML, JSON, etc.). Also includes the character set (like UTF-8) to help the browser display it properly.

#### Server
Example: `Server: nginx`

Shows what kind of server software is handling the request. Good for debugging, but can reveal server information that might be useful for attackers, so many people remove or obscure this one.

### Other Common Response Headers

#### Set-Cookie
Example: `Set-Cookie: sessionId=38af1337es7a8`

Sends cookies from the server to the client, which the client then stores and sends back with future requests. To keep things secure, ensure cookies are set with the HttpOnly flag (so they can't be accessed by JavaScript) and the Secure flag (so they're only sent over HTTPS).

#### Cache-Control
Example: `Cache-Control: max-age=600`

Tells the client how long it can cache the response before checking with the server again. Can also prevent sensitive info from being cached if needed (using no-cache).

#### Location
Example: `Location: /index.html`

Used in redirection (3xx) responses. Tells the client where to go next if the resource has moved. If users can modify this header during requests, validate and sanitize it to avoid open redirect vulnerabilities.

---

## Security Headers

HTTP Security Headers help improve the overall security of web applications by providing mitigations against attacks like Cross-Site Scripting (XSS).

### Content Security Policy (CSP)

Malicious code could be hosted on a separate website or domain and injected into the vulnerable website. A CSP provides a way for administrators to say what domains or sources are considered safe.

**Common CSP directives**:

- **default-src**: Specifies the default policy (e.g., self, meaning only the current website).
- **script-src**: Policy for where scripts can be loaded from (e.g., self and scripts from https://cdn.example.com).
- **style-src**: Policy for where CSS stylesheets can be loaded from (e.g., self).

### HSTS (HTTP Strict Transport Security)

Ensures that web browsers will always connect over HTTPS.

**HSTS directives**:

- **max-age**: Expiry time in seconds for this setting.
- **includeSubDomains**: Optional. Instructs the browser to apply this setting to all subdomains.
- **preload**: Optional. Allows the website to be included in preload lists so browsers enforce HSTS before even visiting the website.

### X-Content-Type-Options

- **nosniff**: Instructs the browser not to sniff or guess the MIME type.

### Referrer-Policy

Controls what referrer information is sent with requests.

- **no-referrer**: Completely disables any information being sent about the referrer.
- **same-origin**: Only sends referrer information when the destination is part of the same origin.
- **strict-origin**: Only sends the referrer as the origin when the protocol stays the same.
- **strict-origin-when-cross-origin**: Similar to strict-origin except for same-origin requests, where it sends the full URL path.

---
# JavaScript Essentials

## Core Concepts

| Term | Definition |
|------|-----------|
| **Function** | A block of code designed to perform a specific task |
| **Loop** | Runs a code block multiple times as long as a condition is true |
| **Conditional** | Executes different code blocks depending on whether a condition is true or false |

---

## Running JavaScript in the Browser

Open the browser console two ways:
- `Ctrl + Shift + I` → Console tab
- Right-click anywhere → Inspect → Console tab

Lets you run JS directly without installing anything.

---

## Internal vs External JavaScript

**Internal JS** — JS written directly inside an HTML file
- Good for beginners, easier to see how script interacts with HTML

**External JS** — JS stored in a separate `.js` file, linked into HTML
- Keeps HTML clean and organised
- To check which a site uses: right-click → View Page Source

---

## Dangerous JS Functions (XSS Risk)

Functions like `alert()`, `prompt()`, and `confirm()` allow developers to display messages and gather input. If implemented insecurely, attackers can exploit these for **Cross-Site Scripting (XSS)** attacks.

**Example — malicious HTML attachment:**
```javascript
// Shows "Hacked" alert 3 times
for (let i = 0; i < 3; i++) {
    alert("Hacked");
}
```

---

## Bypassing Login Forms

If authentication logic is handled in JS (client-side), an attacker can read the source code and extract the credentials directly — no brute force needed.

```javascript
// Bad example — credentials visible in source
if (username === "admin" && password === "secret123") {
    // grant access
}
```

> ⚠️ Never implement real authentication in client-side JavaScript.

---

## Obfuscation & Minification

- **Minification** — removes whitespace and shortens variable names to reduce file size
- **Obfuscation** — transforms readable code into gibberish to make it harder to reverse engineer
- Attackers use online tools to deobfuscate JS and read hidden logic

---

## Security Best Practices

| Rule | Why |
|------|-----|
| Never hardcode API keys or credentials | Users can view source code and extract them |
| Don't handle authentication in JS | Client-side logic can be read and bypassed |
| Sanitise all user input | Prevents XSS attacks via alert/prompt/confirm abuse |
| Use external JS files | Easier to manage and audit |


# SQL Fundamentals
Relational databases: Store structured data, meaning the data inserted into this database follows a structure
Non-relational databases: Instead of storing data the above way, store data in a non-tabular format.
Primary Keys: A primary key is used to ensure that the data collected in a certain column is unique. 
Foreign Keys: A foreign key is a column (or columns) in a table that also exists in another table within the database, and therefore provides a link between the two tables
