1. How to register a protocol for Alamofire
2. How to register multiple protocols
3. How to use URLProtocolClient
4. How to get the body of a request


In this tutorial, we will show you how to use the versatile `URLProtocol` class to intercept requests made by an app, independently of whether the request originated from a `URLSession`, a wrapper library such as Alamofire, an `NSURLConnection`, or an Ajax request inside a web view.

Real-world applications include: 

1. Proxies (forward, or reverse)
2. Stub http requests for testing (see [Mockingjay](https://github.com/kylef-archive/Mockingjay))
3. Network activity indicators (see [Big Brother](https://github.com/marcelofabri/BigBrother))
4. Certificate pinning

### Laying the Groundwork 👷

To get started, create a new Single View Application in Xcode, and select Swift as the language.

Replace the contents of `ViewController` with the following:

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
    "Connection": "close", 
    "Content-Length": "17", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "TuteNSURLProtocol/1 CFNetwork/811.4.18 Darwin/16.3.0"
  }, 
  "json": {
    "hello": "world"
  }, 
  "origin": "190.104.131.250", 
  "url": "https://httpbin.org/post"
}
```

### Intercepting Requests 🕵


This is a good starting point for our proxy which will intercept outgoing requests and wrap them in new object.

Create a new class, called `MyProxyProtocol`, making it a subclass of `URLProtocol` and replace its contents with the following:

```swift
class MyProxyProtocol: URLProtocol {
    
    // #1
    override class func canInit(with request: URLRequest) -> Bool {
        print(#function)
        return true
    }
    
    // #1
    override class func canonicalRequest(for request: URLRequest) -> URLRequest {
        print(#function)
        return request
    }
    
    // #3
    override func startLoading() {
        print(#function)
    }
    
    // #4
    override func stopLoading() {
        print(#function)
    }
}
```

Now, at the beginning of `viewDidLoad` in `ViewController`, register our proxy so that it can intercept requests:

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Register our proxy.
        URLProtocol.registerClass(MyProxyProtocol.self)

		...
```

There are a couple of methods of `URLProtocol` that we must override in our subclass to implement a proxy:

1. `canInit(with:)` - Here we simply return `true` to indicate we want to intercept _all_ requests. A more sophisticated implementation would use the passed in `request` parameter to determine whether the request should be handled or allowed to continue.
2. `canonicalRequest(for:)` - This method is often not understood properly. Simply returning the request is the most common implementation.
3. `startLoading()` and `stopLoading()` - Once a request has been intercepted, `startLoading()` is called and the original request is cancelled. Here you can make a copy of the original request, modify it, and resend it. Effectively, this is where you can modify a request.

This is where the actual proxy is implemented, we'll first define a variable to hold our new data task.

```swift
class MyProxyProtocol: URLProtocol {
    
    var dataTask: URLSessionDataTask?
    ...
```

Now implement `startLoading()` and `stopLoading` like so:

```swift
	override func startLoading() {
	    print(#function)
	    
	    dataTask = URLSession.shared.dataTask(with: request, completionHandler: { data, response, error in
	        
	        if let data = data, let response = response {
	            self.client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .notAllowed)
	            self.client?.urlProtocol(self, didLoad: data)
	        } else if let error = error {
	            self.client?.urlProtocol(self, didFailWithError: error)
	        }
	        self.client?.urlProtocolDidFinishLoading(self)
	    })
	    dataTask?.resume()
	    
	}
	
	override func stopLoading() {
	    self.dataTask?.cancel()
	}
```

This is basically the simplest implementation of `URLProtocol`, that does not quite work! Give the project a run and open the console. You'll see that the `print(#function)` is being hit indefinitely. Why is this so? It's because inside `startLoading()`, we're creating a new request, which again triggers `startLoading()`, and on, and on...

The solution? Tag handled requests so they don't keep on getting handled. Modify `canInit(with:)` and `startLoading()` like so:

```swift
	override class func canInit(with request: URLRequest) -> Bool {
	    if URLProtocol.property(forKey: "example", in: request) as? Bool == true {
	        return false
	    }
	    return true
	}

	override func startLoading() {
	    print(#function)
	    
	    let mutableRequest = (request as NSURLRequest).mutableCopy() as! NSMutableURLRequest
	    URLProtocol.setProperty(true, forKey: "is_handled", in: mutableRequest)
	    let newRequest = mutableRequest as URLRequest
	    
	    dataTask = URLSession.shared.dataTask(with: newRequest, completionHandler: { data, response, error in
	        
	        if let data = data, let response = response {
	            self.client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .notAllowed)
	            self.client?.urlProtocol(self, didLoad: data)
	        } else if let error = error {
	            self.client?.urlProtocol(self, didFailWithError: error)
	        }
	        self.client?.urlProtocolDidFinishLoading(self)
	    })
	    dataTask?.resume()
	}
```

`URLProtocol` provides methods to add arbitrary data to `URLRequest` instances via the `setProperty(_:forKey:in:)` and get the data using `property(forKey:in:)`, and we use these methods to tag requests as handled.

There is a caveat though, see that ugly `NSMutableURLRequest` casting and copying? It turns out that these methods to set and get methods are broken in Swift 3. `setProperty(_:forKey:in:)` inexplicatly requires an `NSMutableURLRequest` instead of a `URLRequest`, although if you simply cast your request to `NSMutableURLRequest`, whatever you try to set is always nil when you retrieve it. The workaround is to cast the request to a Foundation equivalent (NS prefix), and make a mutable copy. Finally, don't forget to pass the mutable copy you just created to the data task. Thanks to [Philippe Hausler's comments on Swift issue SR-2804](https://bugs.swift.org/browse/SR-2804?focusedCommentId=23642&page=com.atlassian.jira.plugin.system.issuetabpanels%3Acomment-tabpanel#comment-23642) for this workaround!
