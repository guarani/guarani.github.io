---
layout: post
title: "TIL: Obj-C NSError** Methods Become Swift Throwing Functions"
comments: true
tags:
- TIL
- Swift
- Obj-C
- Errors
---

## Today I Learned: Obj-C NSError** Methods Become Swift Throwing Functions

I was brushing up on my Obj-C knowledge today when I learned this cool interop feature: Obj-C methods that use `NSError**` for error handling can be called from Swift as throwing functions.

### Objective-C Pattern
```objc
- (BOOL)performOperationWithError:(NSError **)error;
```

### Example

```swift
// Instead of the old Objective-C style:
// var error: NSError?
// let success = someObject.performOperation(&error)
// if !success { /* handle error */ }

// You can now use Swift's try-catch:
do {
    try someObject.performOperation()
} catch {
    print("Operation failed: \(error)")
}
```

Note that the method's return value – which was an indicator of whether or not to check check for an error – is not needed anymore, so Swift smartly discards it.