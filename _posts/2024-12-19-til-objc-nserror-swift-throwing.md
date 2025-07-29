---
layout: post
title: "How Objective-C Error Handling Works in Swift"
date: 2024-12-19 10:00:00 -0300
categories: swift objective-c til
tags: [TIL, Swift, Objective-C, Errors, Interop]
excerpt: "A quick look at how Objective-C methods with NSError** parameters automatically become throwing functions in Swift - showcasing seamless language interoperability."
---

## Today I Learned: Obj-C NSError** Methods Become Swift Throwing Functions

I was brushing up on my Obj-C knowledge today when I learned this cool interop feature: Obj-C methods that use `NSError**` for error handling can be called from Swift as throwing functions.

### Objective-C Pattern
```objc
- (BOOL)performOperationWithError:(NSError **)error;
```

### Swift Transformation

When imported into Swift, this method automatically becomes a throwing function:

```swift
func performOperation() throws
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

### Key Points

1. **Automatic conversion**: No need for manual bridging code
2. **Return value handling**: The BOOL return value is automatically discarded since errors are now handled via exceptions
3. **Seamless interop**: Existing Objective-C libraries work naturally with Swift error handling patterns

This is just one example of how Apple has made the transition between Objective-C and Swift as smooth as possible. The automatic bridging of error handling patterns means legacy Objective-C code feels natural when used from Swift.

### Real-World Example

Here's how this might look with Core Data:

```objc
// Objective-C
- (BOOL)save:(NSError **)error;
```

```swift
// Swift usage
do {
    try managedObjectContext.save()
    print("Successfully saved changes")
} catch {
    print("Save failed: \(error.localizedDescription)")
}
```

Much cleaner than manual error checking!

---

*Part of my ongoing learning series. Small discoveries that make development more enjoyable.* 