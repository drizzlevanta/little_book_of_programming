# CORS
CORS stands for Cross-Origin Resource Sharing.

Imagine you're on a website, let's call it "Website A". In your browser, when Website A tries to make a request (like loading an image, fetching data, or submitting a form) to another website, let's call it "Website B," the browser might stop that request. This security feature is called the "Same-Origin Policy."

Now, CORS comes into play when you actually want Website A to be able to make requests to Website B. CORS is like a set of rules that allows or restricts these kinds of requests between different websites.

## Breakdown:
- Same-Origin Policy: By default, browsers don't allow a web page to make requests to a different domain than the one that served the web page. This is to prevent potentially malicious actions.
- CORS to the Rescue: Sometimes, you want Website A to be able to make requests to Website B. CORS is a way for Website B to say, "Hey, Website A, you're allowed to use my stuff." It does this by adding some extra headers to its responses.
- The CORS Headers: When Website A makes a request to Website B, Website B can include special CORS headers in its response. These headers tell the browser, "It's okay, Website A, you can use my resources." Without these headers, the browser might block the request.
- CORS Middleware in Express: In the context of `Express`, you can use a middleware called cors to easily add these CORS headers to your responses. It's like a helper that takes care of the CORS-related stuff for you.

So, in summary, CORS is a set of rules and headers that allows one website to ask for permission from another website to use its resources. It's like a friendly conversation between websites that helps ensure security on the web while allowing for necessary interactions between different domains.


## Workflow
1. Client (Browser) Sends Request to Server: Website A (client) sends an HTTP request to Website B (server).
2. Browser Blocks Request: The SOP kicks in, and by default, the browser blocks the request from Website A to Website B because they are different origins.
3. CORS Headers in Server's Response: Despite the initial request being blocked, if Website B is configured to support CORS, it includes specific CORS headers in its response. These headers, such as Access-Control-Allow-Origin, Access-Control-Allow-Methods, and others, declare which origins, methods, and headers are allowed to access its resources.
4. Browser Processes Headers: The browser, upon receiving the response with CORS headers, checks if Website A is permitted by the server. If the server allows it, the browser allows the response to be processed, and the requested data/resource may be accessed by the client-side JavaScript code on Website A.

So, it's the server's responsibility to include the appropriate CORS headers in its response to inform the browser about which origins are allowed to access its resources, even though the browser initially blocks the request based on the SOP. This way, CORS provides a controlled way for servers to selectively enable cross-origin requests.