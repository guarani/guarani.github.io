---
layout: post
title: "How to Build a Proxy on iOS"
date: 2017-08-06 10:00:00 -0300
categories: ios swift networking
tags: [iOS, Swift, URLProtocol, Networking, Tutorial]
excerpt: "Learn how to use URLProtocol to intercept network requests in iOS apps, with real-world applications including proxies, testing stubs, and network monitoring."
---

In this tutorial, I'll show you how to use the versatile `URLProtocol` class to intercept network requests inside your app, independently of whether the request originated from a `URLSession`, a wrapper library such as Alamofire, an `NSURLConnection`, or an Ajax request inside a web view.

Real-world applications include: 

1. Proxies
2. Stub HTTP requests for testing (see [Mockingjay](https://github.com/kylef-archive/Mockingjay))
3. Network activity indicators (see [Big Brother](https://github.com/marcelofabri/BigBrother))
4. Certificate pinning

### Laying the Groundwork 👷

To get started, create a new **Single View Application** in Xcode, and select **Swift** as the language.

Replace the contents of `ViewController.swift` with the following:

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        let url = URL(string: "https://httpbin.org/post")!
        var request = URLRequest(url: url)
        request.httpMethod = "POST"
        request.addValue("application/json", forHTTPHeaderField: "Content-Type")
        request.addValue("application/json", forHTTPHeaderField: "Accept")
        request.httpBody = try? JSONSerialization.data(withJSONObject: [ "hello": "world" ], options: [])
        
        URLSession.shared.dataTask(with: request, completionHandler: { data, response, error in
            guard let data = data else { return }
            guard let str = String(data: data, encoding: .utf8) else { return }
            print(str)
        }).resume()
    }
}
``` 

The above code posts `{"hello": "world"}` (a JSON object) to a public test endpoint that simply echos the data back, along with a few other bits of information. Build and run the project, you should see something like this in your console (note that the `json` attribute contains the sent JSON):

```json
{
  "args": {}, 
  "data": "{\"hello\":\"world\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept": "application/json", 
    "Accept-Encoding": "gzip, deflate", 
    "Accept-Language": "en-us", 
    "Content-Length": "17", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "ProxyApp/1.0 CFNetwork/811.5.4 Darwin/16.6.0 (x86_64)"
  }, 
  "json": {
    "hello": "world"
  }, 
  "origin": "186.23.16.226", 
  "url": "https://httpbin.org/post"
}
```

Great! Now that we have a simple HTTP request working, let's create a custom `URLProtocol` to intercept it.

### Building the Custom URLProtocol 🔧

Create a new Swift file called `ProxyURLProtocol.swift` and add the following code:

```swift
import Foundation

class ProxyURLProtocol: URLProtocol {
    
    // MARK: - URLProtocol Implementation
    
    override class func canInit(with request: URLRequest) -> Bool {
        // Only handle HTTP and HTTPS requests
        guard let scheme = request.url?.scheme?.lowercased() else {
            return false
        }
        return scheme == "http" || scheme == "https"
    }
    
    override class func canonicalRequest(for request: URLRequest) -> URLRequest {
        return request
    }
    
    override func startLoading() {
        // Log the original request
        print("🔄 Intercepted request: \(request.url?.absoluteString ?? "unknown")")
        print("   Method: \(request.httpMethod ?? "GET")")
        print("   Headers: \(request.allHTTPHeaderFields ?? [:])")
        
        // Create a new session to perform the actual request
        let session = URLSession(configuration: .default)
        let task = session.dataTask(with: request) { [weak self] data, response, error in
            guard let self = self else { return }
            
            if let error = error {
                self.client?.urlProtocol(self, didFailWithError: error)
            } else {
                if let response = response {
                    self.client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .notAllowed)
                }
                
                if let data = data {
                    print("📦 Response data size: \(data.count) bytes")
                    self.client?.urlProtocol(self, didLoad: data)
                }
                
                self.client?.urlProtocolDidFinishLoading(self)
            }
        }
        
        task.resume()
    }
    
    override func stopLoading() {
        // Clean up if needed
    }
}
```

This custom `URLProtocol` does several important things:

1. **`canInit(with:)`** - Determines which requests this protocol should handle. We're only intercepting HTTP/HTTPS requests.
2. **`canonicalRequest(for:)`** - Returns the canonical version of the request. For our proxy, we return the request unchanged.
3. **`startLoading()`** - This is where the magic happens. We log the request details, create a new `URLSession` to perform the actual network call, and forward the response back to the original caller.
4. **`stopLoading()`** - Clean up any resources when the request is cancelled.

### Registering the Protocol 📝

Now we need to register our custom protocol with the URL loading system. Update your `ViewController.swift`:

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Register our custom protocol
        URLProtocol.registerClass(ProxyURLProtocol.self)
        
        let url = URL(string: "https://httpbin.org/post")!
        var request = URLRequest(url: url)
        request.httpMethod = "POST"
        request.addValue("application/json", forHTTPHeaderField: "Content-Type")
        request.addValue("application/json", forHTTPHeaderField: "Accept")
        request.httpBody = try? JSONSerialization.data(withJSONObject: [ "hello": "world" ], options: [])
        
        URLSession.shared.dataTask(with: request, completionHandler: { data, response, error in
            guard let data = data else { return }
            guard let str = String(data: data, encoding: .utf8) else { return }
            print("✅ Final response: \(str)")
        }).resume()
    }
}
```

Run the app again, and you should see the proxy logging output in the console:

```
🔄 Intercepted request: https://httpbin.org/post
   Method: POST
   Headers: ["Accept": "application/json", "Content-Type": "application/json"]
📦 Response data size: 426 bytes
✅ Final response: {
  "args": {}, 
  "data": "{\"hello\":\"world\"}", 
  ...
}
```

Perfect! Our proxy is now intercepting and logging all network requests.

### Advanced Use Cases 🚀

Now that we have the basic proxy working, let's explore some advanced use cases:

#### 1. Request Modification

You can modify requests before they're sent:

```swift
override func startLoading() {
    // Create a mutable copy of the request
    let mutableRequest = request.mutableCopy() as! NSMutableURLRequest
    
    // Add custom headers
    mutableRequest.setValue("ProxyApp/1.0", forHTTPHeaderField: "User-Agent")
    mutableRequest.setValue("Bearer your-token-here", forHTTPHeaderField: "Authorization")
    
    print("🔄 Modified request: \(mutableRequest.url?.absoluteString ?? "unknown")")
    
    // Use the modified request
    let session = URLSession(configuration: .default)
    let task = session.dataTask(with: mutableRequest as URLRequest) { [weak self] data, response, error in
        // ... rest of the implementation
    }
    
    task.resume()
}
```

#### 2. Response Modification

You can also modify responses before they reach your app:

```swift
override func startLoading() {
    let session = URLSession(configuration: .default)
    let task = session.dataTask(with: request) { [weak self] data, response, error in
        guard let self = self else { return }
        
        if let error = error {
            self.client?.urlProtocol(self, didFailWithError: error)
        } else {
            if let response = response {
                self.client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .notAllowed)
            }
            
            if var data = data {
                // Modify the response data
                if let jsonObject = try? JSONSerialization.jsonObject(with: data, options: []) as? [String: Any] {
                    var modifiedJson = jsonObject
                    modifiedJson["proxy_timestamp"] = Date().timeIntervalSince1970
                    
                    if let modifiedData = try? JSONSerialization.data(withJSONObject: modifiedJson, options: []) {
                        data = modifiedData
                    }
                }
                
                print("📦 Modified response data size: \(data.count) bytes")
                self.client?.urlProtocol(self, didLoad: data)
            }
            
            self.client?.urlProtocolDidFinishLoading(self)
        }
    }
    
    task.resume()
}
```

#### 3. Conditional Interception

You might want to only intercept certain requests:

```swift
override class func canInit(with request: URLRequest) -> Bool {
    // Only intercept requests to specific domains
    guard let host = request.url?.host else { return false }
    
    let targetHosts = ["api.myapp.com", "httpbin.org"]
    return targetHosts.contains(host)
}
```

#### 4. Request Caching

Implement custom caching logic:

```swift
class CachingProxyURLProtocol: URLProtocol {
    static var cache: [String: Data] = [:]
    
    override func startLoading() {
        let cacheKey = request.url?.absoluteString ?? ""
        
        // Check cache first
        if let cachedData = CachingProxyURLProtocol.cache[cacheKey] {
            print("📋 Serving from cache: \(cacheKey)")
            
            // Create a mock response
            let response = HTTPURLResponse(url: request.url!, statusCode: 200, httpVersion: nil, headerFields: nil)!
            client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .notAllowed)
            client?.urlProtocol(self, didLoad: cachedData)
            client?.urlProtocolDidFinishLoading(self)
            return
        }
        
        // Not in cache, make the request
        let session = URLSession(configuration: .default)
        let task = session.dataTask(with: request) { [weak self] data, response, error in
            guard let self = self else { return }
            
            if let error = error {
                self.client?.urlProtocol(self, didFailWithError: error)
            } else {
                if let response = response {
                    self.client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .notAllowed)
                }
                
                if let data = data {
                    // Cache the response
                    CachingProxyURLProtocol.cache[cacheKey] = data
                    print("💾 Cached response for: \(cacheKey)")
                    
                    self.client?.urlProtocol(self, didLoad: data)
                }
                
                self.client?.urlProtocolDidFinishLoading(self)
            }
        }
        
        task.resume()
    }
}
```

### Testing and Debugging 🐛

`URLProtocol` is particularly useful for testing:

```swift
class MockURLProtocol: URLProtocol {
    static var mockData: [String: Data] = [:]
    static var mockError: Error?
    
    override class func canInit(with request: URLRequest) -> Bool {
        return true  // Handle all requests in test mode
    }
    
    override class func canonicalRequest(for request: URLRequest) -> URLRequest {
        return request
    }
    
    override func startLoading() {
        let url = request.url?.absoluteString ?? ""
        
        // Check if we should return an error
        if let error = MockURLProtocol.mockError {
            client?.urlProtocol(self, didFailWithError: error)
            return
        }
        
        // Return mock data if available
        if let data = MockURLProtocol.mockData[url] {
            let response = HTTPURLResponse(url: request.url!, statusCode: 200, httpVersion: nil, headerFields: nil)!
            client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .notAllowed)
            client?.urlProtocol(self, didLoad: data)
            client?.urlProtocolDidFinishLoading(self)
        } else {
            // Return empty response
            let response = HTTPURLResponse(url: request.url!, statusCode: 404, httpVersion: nil, headerFields: nil)!
            client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .notAllowed)
            client?.urlProtocolDidFinishLoading(self)
        }
    }
    
    override func stopLoading() {
        // Nothing to do
    }
}

// In your test setup:
MockURLProtocol.mockData["https://api.myapp.com/users"] = """
    {"users": [{"id": 1, "name": "Test User"}]}
    """.data(using: .utf8)!
URLProtocol.registerClass(MockURLProtocol.self)
```

### Key Takeaways 🎯

1. **`URLProtocol` is powerful**: It can intercept any network request made by your app, regardless of the networking library used.

2. **Registration order matters**: The last registered protocol is checked first. If multiple protocols can handle the same request, the most recently registered one wins.

3. **Don't forget to unregister**: Use `URLProtocol.unregisterClass()` when you no longer need the protocol, especially in test scenarios.

4. **Be careful with infinite loops**: If your protocol makes network requests, make sure they don't get intercepted by the same protocol again.

5. **Performance considerations**: Interception adds overhead. Only intercept requests when necessary.

### Conclusion 🏁

`URLProtocol` is a versatile tool that opens up many possibilities for network handling in iOS apps. Whether you're building a proxy, implementing custom caching, stubbing requests for testing, or adding network monitoring, `URLProtocol` provides the foundation you need.

The complete source code for this tutorial is available on [GitHub](https://github.com/username/ios-proxy-tutorial).

**What will you build with `URLProtocol`?** Let me know in the comments below!

---

*This post was originally published in 2017 and demonstrates networking concepts that remain relevant for modern iOS development.* 