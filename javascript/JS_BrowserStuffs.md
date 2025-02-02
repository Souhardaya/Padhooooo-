<!-- TOC -->

- [Understanding CORS](#understanding-cors)
  - [But why do you need these ?](#but-why-do-you-need-these-)
  - [Understanding how stuffs work](#understanding-how-stuffs-work)
  - [Questions on CORS](#questions-on-cors)
    - [What is CORS, and why is it important for web security?](#what-is-cors-and-why-is-it-important-for-web-security)
    - [Explain the Same-Origin Policy and how CORS relaxes it.](#explain-the-same-origin-policy-and-how-cors-relaxes-it)
    - [When does a web browser initiate a preflight request?](#when-does-a-web-browser-initiate-a-preflight-request)
    - [What are simple cross-origin requests, and when are preflight requests not required?](#what-are-simple-cross-origin-requests-and-when-are-preflight-requests-not-required)
    - [What HTTP headers are involved in a CORS preflight request, and what information do they carry?](#what-http-headers-are-involved-in-a-cors-preflight-request-and-what-information-do-they-carry)
    - [How can a server respond to a preflight request to allow cross-origin access?](#how-can-a-server-respond-to-a-preflight-request-to-allow-cross-origin-access)
    - [What are the potential security risks of not implementing CORS correctly?](#what-are-the-potential-security-risks-of-not-implementing-cors-correctly)
    - [How can you enable CORS in different server-side environments (e.g., Node.js, Apache, or Nginx)?](#how-can-you-enable-cors-in-different-server-side-environments-eg-nodejs-apache-or-nginx)
    - [Explain the difference between a simple and a non-simple cross-origin request.](#explain-the-difference-between-a-simple-and-a-non-simple-cross-origin-request)
    - [How do you debug and troubleshoot CORS-related issues in a web application?](#how-do-you-debug-and-troubleshoot-cors-related-issues-in-a-web-application)
- [What is the easiest multi condition checking](#what-is-the-easiest-multi-condition-checking)
- [How do you capture browser back button](#how-do-you-capture-browser-back-button)
- [How do you disable right click in the web page](#how-do-you-disable-right-click-in-the-web-page)
- [What are wrapper objects](#what-are-wrapper-objects)
- [What is AJAX](#what-is-ajax)
- [What is the difference between shim and polyfill](#what-is-the-difference-between-shim-and-polyfill)
- [What is babel](#what-is-babel)
- [Is Node.js completely single threaded](#is-nodejs-completely-single-threaded)

<!-- /TOC -->


# Understanding CORS

CORS (Cross-Origin Resource Sharing) is needed to allow Website A to ask for permission from Website B to access its information. It's like Website A saying, "Hey Website B, can I please see your data?". CORS helps ensure that Website B is okay with this request and allows Website A to access the data if everything checks out.

So, CORS helps keep things secure on the web while still allowing websites to communicate and share information with each other when needed.

## But why do you need these ?

The CORS policy is in place to enhance security on the web and protect users from potential risks that could arise from cross-origin requests.

- **Security**: Without CORS, any website could potentially make requests to other websites on behalf of the user (e.g., reading sensitive data, performing actions, etc.). This could be exploited by malicious websites to steal data or perform harmful actions without the user's knowledge.

- **Privacy**: CORS helps prevent unauthorized access to a user's private data. It ensures that only websites from trusted origins (domains) can access certain resources, like personal information or user accounts.

- **Isolation of Websites**: CORS helps maintain the separation between different websites and their resources. Each website operates in its own "sandbox," and CORS ensures that one website cannot freely interact with or modify resources from another website.

- **Protection against CSRF**: Cross-Site Request Forgery (CSRF) attacks are thwarted by CORS. It prevents attackers from forcing a user's browser to make requests to other websites where the user is authenticated, potentially performing unwanted actions on those sites.

By enforcing the CORS policy, browsers ensure that websites can only interact with resources from their own domain unless explicitly permitted by the server hosting those resources. This way, user data and web interactions remain more secure and controlled, reducing the risk of unauthorized access or malicious activities.

## Understanding how stuffs work

Suppose we have 2 origins, `Origin A` and `Origin B`, so when a request is made from `Origin A` to `Origin B`, the browser will first send a preflight request to `Origin B` to check if it is safe to send the actual request. If `Origin B` allows the preflight request, then the browser will send the actual request to `Origin B`.

The preflight request is an HTTP OPTIONS request that contains the following headers:

- **Access-Control-Request-Method**: The HTTP method of the actual request.

- **Access-Control-Request-Headers**: The headers that will be sent with the actual request.

- **Acess-control-allow-origin**: The origin of the request.

- **Host**: The host of the request.

- **User-Agent**: The user agent of the request.

- **Connection**: The connection of the request.

- **Accept**: The accept of the request.

- **Accept-Encoding**: The accept encoding of the request.


**So does the preflight request is made everytime ?**

No, the preflight request is not made every time for every cross-origin request. It is only made in certain situations, depending on the nature of the cross-origin request and the server's CORS configuration.

The preflight request is made when a cross-origin request meets certain conditions:

- HTTP Methods: If the cross-origin request uses certain HTTP methods other than simple methods, like GET, POST, or HEAD. For example, if the request uses PUT, DELETE, or custom methods, the browser will send a preflight request to check if the server allows the actual request.

- Custom Headers: If the cross-origin request includes custom HTTP headers (non-standard headers), the browser will also trigger a preflight request to verify if the server permits the use of these custom headers in the actual request.

For simple cross-origin requests (e.g., GET or POST without custom headers), the browser doesn't send a preflight request. Instead, it directly sends the actual request to the server, and the server includes appropriate CORS headers in the response, allowing or denying access based on its CORS configuration.

The preflight request helps the server make an informed decision on whether to allow or deny the cross-origin request, ensuring security and protecting against unauthorized access to resources. Once the server responds with the necessary headers during the preflight, subsequent cross-origin requests can be made without the need for additional preflight requests as long as they meet the criteria for simple requests.

## Questions on CORS


### What is CORS, and why is it important for web security?

CORS stands for Cross-Origin Resource Sharing. It is a security feature implemented by web browsers to restrict web pages from making requests to a different domain, protocol, or port (origin) than the one from which they were served. It is crucial for web security because it prevents unauthorized access to sensitive data and resources on other websites, protecting users from potential malicious activities.

### Explain the Same-Origin Policy and how CORS relaxes it.

The Same-Origin Policy is a security feature that browsers follow, allowing web pages to only request resources from the same origin as the one from which the web page was served. CORS relaxes this policy by introducing a mechanism that enables servers to specify which other origins are allowed to access their resources. By using specific HTTP headers in the server's response, CORS allows cross-origin requests in a controlled and secure manner.

### When does a web browser initiate a preflight request?

A web browser initiates a preflight request when a cross-origin request meets certain conditions:
- The request uses HTTP methods other than GET, POST, or HEAD (e.g., PUT, DELETE).
- The request includes custom HTTP headers.


### What are simple cross-origin requests, and when are preflight requests not required?

Simple cross-origin requests are requests that meet the following criteria:
- Use only safe methods (GET, POST, HEAD).
- Do not include custom HTTP headers.
- 
Preflight requests are not required for simple cross-origin requests. The browser directly sends the actual request, and the server responds with appropriate CORS headers to permit or deny access.

### What HTTP headers are involved in a CORS preflight request, and what information do they carry?

The HTTP headers involved in a CORS preflight request are:
 - Origin: Indicates the origin of the web page making the request.
 - Access-Control-Request-Method: Specifies the HTTP method that will be used in the actual request.
 - Access-Control-Request-Headers: Lists the custom headers that will be used in the actual request.

### How can a server respond to a preflight request to allow cross-origin access?

The server can respond to a preflight request by including specific CORS headers in the response. The key header is:
 - Access-Control-Allow-Origin: Specifies the origins that are allowed to access the resource. It can be set to a specific origin or "*" (wildcard) to allow any origin.
 - Access-Control-Allow-Methods: Indicates the HTTP methods allowed for the actual request.
 - Access-Control-Allow-Headers: Lists the custom headers that are allowed in the actual request.

### What are the potential security risks of not implementing CORS correctly?

Not implementing CORS correctly can lead to security vulnerabilities like Cross-Site Request Forgery (CSRF) attacks, where malicious websites trick users into performing actions on other trusted websites without their consent. It can also expose sensitive data to unauthorized domains, leading to data breaches and privacy violations.

### How can you enable CORS in different server-side environments (e.g., Node.js, Apache, or Nginx)?

Enabling CORS in different server-side environments involves adding appropriate headers to the server's responses. For example, in Node.js, you can use the "cors" middleware, in Apache, you can use the "Header" directive, and in Nginx, you can use the "add_header" directive.

### Explain the difference between a simple and a non-simple cross-origin request.

A simple cross-origin request meets the criteria of using only safe methods (GET, POST, HEAD) and not including custom headers. In contrast, a non-simple cross-origin request uses methods like PUT, DELETE, or includes custom headers, which triggers a preflight request before the actual request is made.

### How do you debug and troubleshoot CORS-related issues in a web application?

CORS-related issues can be debugged using browser developer tools. The candidate may mention checking the browser console for error messages, inspecting network requests to see if CORS headers are set correctly, and using tools like Postman or cURL to test API responses directly and verify CORS headers. Additionally, the candidate might suggest reviewing server-side configurations to ensure the appropriate CORS headers are being added to responses.


# What is the easiest multi condition checking

You can use `indexOf` to compare input with multiple values instead of checking each value as one condition.

```javascript
// Verbose approach
if (
input === "first" ||
input === 1 ||
input === "second" ||
input === 2
) {
someFunction();
}
// Shortcut
if (["first", 1, "second", 2].indexOf(input) !== -1) {
 someFunction();
}
```

    

# How do you capture browser back button

The `window.onbeforeunload` method is used to capture browser back button events. This is helpful to warn users about losing the current data.

```javascript
window.onbeforeunload = function () {
alert("You work will be lost");
};
```

    

# How do you disable right click in the web page

The right click on the page can be disabled by returning false from the `oncontextmenu` attribute on the body element.

```html
<body oncontextmenu="return false;"></body>
```

    

# What are wrapper objects

Primitive Values like string, number and boolean don't have properties and methods but they are temporarily converted or coerced to an object(Wrapper object) when you try to perform actions on them. For example, if you apply toUpperCase() method on a primitive string value, it does not throw an error but returns uppercase of the string.

```javascript
let name = "john";

console.log(name.toUpperCase());
```


Behind the scenes treated as `console.log(new String(name).toUpperCase());`

i.e, Every primitive except null and undefined have Wrapper Objects and the list of wrapper objects are String,Number,Boolean,Symbol and BigInt.

    

# What is AJAX

AJAX stands for Asynchronous JavaScript and XML and it is a group of related technologies(HTML, CSS, JavaScript, XMLHttpRequest API etc) used to display data asynchronously. i.e. We can send data to the server and get data from the server without reloading the web page.

# What is the difference between shim and polyfill

A shim is a library that brings a new API to an older environment, using only the means of that environment. It isn't necessarily restricted to a web application. For example, es5-shim.js is used to emulate ES5 features on older browsers (mainly pre IE9).
     
Whereas polyfill is a piece of code (or plugin) that provides the technology that you, the developer, expect the browser to provide natively.
     
In a simple sentence, A polyfill is a shim for a browser API.
    

# What is babel

Babel is a JavaScript transpiler to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environments. Some of the main features are listed below,

1. Transform syntax
2. Polyfill features that are missing in your target environment (using @babel/polyfill)
3. Source code transformations (or codemods)

    

# Is Node.js completely single threaded

Node is a single thread, but some of the functions included in the Node.js standard library(e.g, fs module functions) are not single threaded. i.e, Their logic runs outside of the Node.js single thread to improve the speed and performance of a program.

 

